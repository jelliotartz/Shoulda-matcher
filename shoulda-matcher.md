
# Rails gem: shoulda-matcher


# What does it do?

Shoulda Matchers provides RSpec- and Minitest-compatible one-liners that test common Rails functionality. These tests would otherwise be much longer, more complex, and error-prone.

The Shoulda-matcher gem doesn't provide new functionality- it makes the things that you already have to do easier. 

Some websites where you can find more info:
[github.com/thoughtbot/shoulda-matchers](https://github.com/thoughtbot/shoulda-matchers)
-------
[matchers.shoulda.io](http://matchers.shoulda.io)
-------

# Where Does the Code Go?

spec folder
	- model specs: ActiveModel (validations) & ActiveRecord
	- controller specs: ActionController
  - spec folders and files are automatically generated when you run rails generate

```
├── spec
│   ├── controllers
│   │   ├── articles_controller_spec.rb
│   │   ├── authors_controller_spec.rb
│   ├── models
│   │   ├── article_spec.rb
│   │   ├── author_spec.rb
etc...
```




# How to Install

configuration:

gemfile: `gem 'shoulda-matchers'` 

add to your spec/rails_helper:

```ruby  

require 'shoulda/matchers'

Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    # Choose a test framework:
    with.test_framework :rspec
    with.library :rails
  end
end
```

##### don't forget to bundle!

This is the configuration I used, there are other options available on the [github site](https://github.com/thoughtbot/shoulda-matchers)


# Using it in a Rails App

### Model associations

```ruby
RSpec.describe Article, type: :model do
  it { should validate_presence_of(:title) }
  it { should validate_presence_of(:body) }

  it 'should have associations' do
    should have_many(:comments)
    should have_many(:taggings)
    should have_many(:tags).through(:taggings)
  end
end
```

### Controller spec

```ruby
RSpec.describe ArticlesController, type: :controller do

  describe '#show' do
    article = FactoryGirl.create(:article)

    before(:each) { get :show, {id: article.id} }

    # this is traditional rspec
    it 'renders the show template' do
      expect(response).to render_template('show')
    end

    # same test with shoulda-matcher
    it { should render_template('show') }
  end
end

```

