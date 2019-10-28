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

## Good

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

A better alternative is to use a Query object.


````ruby
class ApplicationQuery
  class << self
    delegate :call, to: :new
  end
end

module WordpressBlogPosts
  class RecentPostsQuery < ApplicationQuery

    attr_reader :relation

    def initialize(base_relation=nil)
      @relation = base_relation || WordpressBlogPost.includes(:wordpress_blog_post_category, :wordpress_author)
    end

    def call
      relation.where.not(wordpress_blog_post_category_id: WordpressBlogPostCategory.
        invalid_wordpress_blog_post_category.select(:id)).
        order(created_at: :desc).
        references(:wordpress_blog_post_category)
    end
  end
end

class WordpressBlogPost < ActiveRecord::Base
  scope :recent_posts, WordpressBlogPosts::RecentPostsQuery
end
````
