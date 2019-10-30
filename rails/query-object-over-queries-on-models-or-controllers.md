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
class RecentWordpressBlogPostQuery < ApplicationQuery

  def initialize(relation = nil)
    @relation = relation 
  end

  def all
    base_relation.not_excluded.ordered
  end

  private

  attr_reader :relation

  def ordered
    not_excluded.order(created_at: :desc)
  end

  def not_excluded
    base_relation.where.not(wordpress_blog_post_category_id: [1, 2, 3])
  end

  def base_relation
    relation || WordpressBlogPost.includes(:wordpress_blog_post_category, :wordpress_author)
  end
end


class WordpressPopularBlogPostsController < ApplicationController
  def index
    @posts = RecentWordpressBlogPostQuery.all
  end
end
````
