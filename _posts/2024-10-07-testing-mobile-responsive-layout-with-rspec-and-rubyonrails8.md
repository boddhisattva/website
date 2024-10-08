---
layout: post
title: How to test mobile responsive layout using Ruby on Rails 8 and RSpec System Tests
date:  2024-10-07 08:54:00 +02:00
categories:
  - rspec
  - system-tests
  - rails8
  - mobile-tests-with-rails
  - capybara
  - selenium-webdriver
---

- The below Rspec code shows testing the navigation to the sign up page using the [mobile's burger menu](https://icons8.de/icons/set/hamburger-menu)(**[see below screenshots](#mobile-screenshots)**)
  - **Post running the mobile spec(s), one needs to resize to normal window size defaults** for running other web app tests
  
- **The Rails 8 app** with the below code is available [here](https://github.com/boddhisattva/learner-web/blob/upgrade_to_rails8/spec/system/mobile/mobile_users_authentication_flow_spec.rb)
  - **Update**: Reverted Rails 8 deploy as some new dependencies don't work currently with Rails 8.0.0.beta1. More details [here](https://github.com/boddhisattva/learner-web/pull/22/commits/36a5e5646c6bc11421400bd5e25c35030203f82f)
  
```ruby
# frozen_string_literal: true

require 'rails_helper'

module Mobile
  describe 'User sign up flow', type: :system do
    describe 'Mobile User sign up flow' do
      let(:activity_day) { create(:activity_day) }

      before do
        page.current_window.resize_to(501, 764) # Resize window to a size similar to that of mobile devices
      end

      after do
        page.current_window.resize_to(1200, 815) # Resize to normal window size defaults
      end

      it 'can access mobile sign up page via burger menu' do
        visit root_path

        find(".navbar-burger").click
        click_on I18n.t("shared.navbar.sign_up")

        expect(page).to have_current_path(new_user_registration_path)
      end
    end
  end
end
```

# Mobile Screenshots

- **App home page** on Mobile

![Mobile responsive layout app homepage with Rails 8](https://i.imgur.com/XnoQZ7I.jpeg)

- Sign Up button shows on clicking the mobile [burger menu](https://icons8.de/icons/set/hamburger-menu)

  ![Sign up button shows on clicking the mobile burger menu](https://i.imgur.com/hdh0IKS.jpeg)

- **Sign up page** on Mobile

  ![Sign up page on Mobile with Rails 8](https://i.imgur.com/9zAklr4.jpeg)
