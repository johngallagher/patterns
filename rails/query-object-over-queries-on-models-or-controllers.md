# Avoid querying sets of objects from models/controllers

Consider extracting complex SQL queries into their own classes, which would otherwise clutter ActiveRecord models or controllers.

# In Practice

Query object goes in `app/queries` folder.



## Bad

````ruby
class WordpressPopularBlogPostsController < ApplicationController
  def index
    @popular_blog_posts = WordpressBlogPost.includes(:wordpress_blog_post_category, :wordpress_author).
                          where.not(wordpress_blog_post_category_id: WordpressBlogPostCategory.
                          invalid_wordpress_blog_post_category.select(:id)).
                          order(created_at: :desc).
                          references(:wordpress_blog_post_category).
                          paginate page: params[:page], per_page: 12
  end
end
````

## Better

````ruby
class WordpressPopularBlogPostsController < ApplicationController
  def index
    @posts = WordpressBlogPost.recent_posts
  end
end

class WordpressBlogPost < ActiveRecord::Base
  scope :recent_posts, -> do
    includes(:wordpress_blog_post_category, :wordpress_author).
      where.not(wordpress_blog_post_category_id: WordpressBlogPostCategory.
      invalid_wordpress_blog_post_category.select(:id)).
      order(created_at: :desc).
      references(:wordpress_blog_post_category).
  end
end
````

## Good

````ruby
class ApplicationQuery
  def self.all
    new.all
  end
end

class RecentWordpressBlogPostQuery < ApplicationQuery
  def all
    WordpressBlogPost.includes(:wordpress_blog_post_category, :wordpress_author).
      not_hidden.
      where.not(wordpress_blog_post_category_id: invalid_categories).
      order(created_at: :desc).
      references(:wordpress_blog_post_category)
  end

  private

  def invalid_categories
    WordpressBlogPostCategory.invalid_wordpress_blog_post_category.select(:id)
  end
end

class WordpressPopularBlogPostsController < ApplicationController
  def index
    @posts = RecentWordpressBlogPostQuery.all
  end
end
````
