---
categories: Rails
name: Avoid building ActiveRecord queries outside of models
---

Prefer scopes or explicit methods defined on the model class. The purpose of the model is to represent records in, and the schema of, the database - so keeping query logic in the model is appropriate.

A good alternative is to define scopes on the model, which are exposed as class methods, or to define the class methods yourself in some cases. Another potential solution is a Query Object.

## Bad

```ruby
def PostsController < ApplicationController
  def index
    @posts = BlogPost.
             where(active: true).
             where("created_at > ", 2.weeks.ago).
             where(public: true)
  end
end
```

## Good

```ruby
def PostsController < ApplicationController
  def index
    @posts = BlogPost.active.recent.visible
  end
end

class BlogPost < ActiveRecord::Base
  scope :active, -> { where(active: true) }
  scope :recent, -> { where("created_at > ", 2.weeks.ago) }
  scope :visible, -> { where(public: true) }
end
```

```ruby
def PostsController < ApplicationController
  def index
    @posts = BlogPost.recently_published
  end

  class BlogPost < ActiveRecord::Base
    scope :active, -> { where(active: true) }
    scope :recent, -> { where("created_at > ", 2.weeks.ago) }
    scope :visible, -> { where(public: true) }
    scope :recently_published, -> { active.recent.visible }
  end
end
```
