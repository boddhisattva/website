---
layout: post
title: How to test application helper code with RSpec
date:  2022-08-15 07:02:00 +02:00
categories:
  - rspec
  - helper
  - helper specs
  - rails
  - testing
  - ruby
---

- You **don’t have to explicitly include** the `ApplicationHelper` module(via `include ApplicationHelper`) in the relevant spec to test it
  
  
  Sample Code in `application_helper.rb`
  
  ```ruby
  module ApplicationHelper
    def title
      return t("learner") unless content_for?(:title)
  
      "#{content_for(:title)} | #{t("learner")}"
    end
  end
  
  ```
  
  Rspec code in `application_helper_spec.rb` that works:
  
  ```ruby
  require 'rails_helper'
  
  RSpec.describe ApplicationHelper, type: :helper do
  
    describe '#title' do
      it 'page specific title in the context of the app name' do
        content_for(:title) { "Page Title" }
  
        expect(title).to eq("Page Title | #{I18n.t('learner')}")
      end
  
      it 'returns app name when page title is missing' do
        expect(title).to eq("#{I18n.t('learner')}")
      end
    end
  end
  
  ```
