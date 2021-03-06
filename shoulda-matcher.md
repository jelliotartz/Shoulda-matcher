
# Rails gem: Shoulda-Matchers


# What Does it Do?

Shoulda Matchers provides RSpec- and Minitest-compatible one-liners that test common Rails functionality. These tests would otherwise be longer, more complex, and error-prone.

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

##### Don't Forget to bundle!

This is the configuration I used. There are other options available on shoulda-matcher's [github site](https://github.com/thoughtbot/shoulda-matchers)


# Using it in a Rails App

### Model Associations

```ruby
RSpec.describe Article, type: :model do

  # testing associations with Shoulda Matcher
  it 'should have many comments' do
    should have_many(:comments)
  end

  # testing associations without Shoulda Matcher
  it "should have many comments" do
    a = Article.reflect_on_association(:comments)
    expect(a.macro).to be(:has_many)
  end

  # testing validations with Shoulda Matchers 
  it { should validate_presence_of(:title) }
  it { should validate_presence_of(:body) }

  # testing has_many/through association with Shoulda Matcher
  it 'has many tags, through taggings' do
    should have_many(:tags).through(:taggings)
  end
end
```

### Controller Spec

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

