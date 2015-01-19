# Reponders, has_scope and new way to code a controller

TL;DR Don't use `inherited_resources`, use `reponders` (with `has_scope`) instead

## Responders
A set of responders modules to dry up your Rails 3.2+ app. Basiclly it contains logic to repond to a request in rails, maybe render a flash, or redirect to correct page ...

Add it to your Gemfile

```ruby
gem 'responders'
```

Now try to generate a scaffold a `Post` mode, which has `title` and `content`

```bash
rails generate scaffold Post title content
```

Navigate to PostsController and you will see

```ruby
class PostsController < ApplicationController
  before_action :set_post, only: [:show, :edit, :update, :destroy]

  respond_to :html

  def index
    @posts = Post.all
    respond_with(@posts)
  end

  def show
    respond_with(@post)
  end

  def new
    @post = Post.new
    respond_with(@post)
  end

  def edit
  end

  def create
    @post = Post.new(post_params)
    @post.save
    respond_with(@post)
  end

  def update
    @post.update(post_params)
    respond_with(@post)
  end

  def destroy
    @post.destroy
    respond_with(@post)
  end

  private
    def set_post
      @post = Post.find(params[:id])
    end

    def post_params
      params.require(:post).permit(:title, :content)
    end
end
```

At the end of each action, you just need to call `respond_with` with repond object. It will know whether your record create (update) success or not and render correct page with correct notice or error flash.

## Has_scope
Add to Gemfile

```ruby
gem 'has_scope'
```

Now in your controller, you can filter using the same name with model scope

Example:
```ruby
class Graduation < ActiveRecord::Base
  scope :featured, -> { where(:featured => true) }
  scope :by_degree, -> degree { where(:degree => degree) }
  scope :by_period, -> started_at, ended_at { where("started_at = ? AND ended_at = ?", started_at, ended_at) }
end
```

```ruby
class GraduationsController < ApplicationController
  has_scope :featured, :type => :boolean
  has_scope :by_degree

  def index
    @graduations = apply_scopes(Graduation).all
  end
end
```

## Bonus
If you use (and you should use) `kaminari` to paging, you can combine it with 2 gem above and have a nice, clean and easy to understand `PostsController`

```ruby
class Post < ActiveRecord::Base
  scope :order, -> (type) { order(created_at: type)}
end
```

```ruby
class PostsController < ApplicationController
  before_action :set_post, only: [:show, :edit, :update, :destroy]
  has_scope :page, default: 1
  has_scope :per, default: 10
  has_scope :order, default: :asc
  respond_to :html

  def index
    @posts = apply_scopes(Post).all
    respond_with(@posts)
  end

  def show
    respond_with(@post)
  end

  def new
    @post = Post.new
    respond_with(@post)
  end

  def edit
  end

  def create
    @post = Post.new(post_params)
    @post.save
    respond_with(@post)
  end

  def update
    @post.update(post_params)
    respond_with(@post)
  end

  def destroy
    @post.destroy
    respond_with(@post)
  end

  private
    def set_post
      @post = Post.find(params[:id])
    end

    def post_params
      params.require(:post).permit(:title, :content)
    end
end
```

```
/posts 
#=> return page 1 with 10 items, order `asc` by created_at

/posts?page=2&per=20 
#=> return page 2 with 20 items

/posts?order=desc
#=> return page 1 with 10 items, order `desc` by created_at
```
