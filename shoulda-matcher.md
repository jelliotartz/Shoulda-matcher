
# Rails gem: shoulda-matcher


# What does it do?

Shoulda Matchers provides RSpec- and Minitest-compatible one-liners that test common Rails functionality. These tests would otherwise be much longer, more complex, and error-prone.

The Shoulda-matcher gem doesn't provide new functionality- it makes the things that you already have to do easier. 


[github.com/thoughtbot/shoulda-matchers]:(https://github.com/thoughtbot/shoulda-matchers)
[matchers.shoulda.io]:(http://matchers.shoulda.io)

# Where does the code go? (file/folders)

spec folder
	- model specs: ActiveModel (validations) & ActiveRecord
	- controller ActionController (can check with whether conroller calls view renders correctly. view spec is handled by Capybara)

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

spec folder files are automatically generated when you run rails generate...


# How to install or set it up

configuration:

gemfile: `gem 'shoulda-matchers'` (don't forget to bundle!)

```ruby  
# rails-helper

require 'shoulda/matchers'

Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    # Choose a test framework:
    with.test_framework :rspec
    with.library :rails
  end
end
```

This is the configuration I used, there are other options available on the [github site](https://github.com/thoughtbot/shoulda-matchers)


# Using it in a Rails app and showing code

1. Model associations

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

2. Controller spec

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

