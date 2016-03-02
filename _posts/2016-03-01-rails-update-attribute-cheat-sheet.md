---
layout: post
title: "Rails update attributes methods cheat sheet"
date: 2016-03-01
categories: ruby
---

> Different ways to set or update attribute in Rails

## Rails 4

  Method | User default accessor | Saved to DB | Validitions | Callbacks | Touches updated_at | Readonly Check
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [attribute=](http://apidock.com/rails/ActiveRecord/AttributeMethods/Write/attribute)| Yes | No | *n/a* | *n/a* | *n/a* | *n/a*
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [attributes=](http://apidock.com/rails/ActiveRecord/AttributeAssignment/attributes%3D)| Yes | No | *n/a* | *n/a* | *n/a* | *n/a*
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [write_attribute](http://apidock.com/rails/ActiveRecord/AttributeMethods/Write/write_attribute)| No | No | *n/a* | *n/a* | *n/a* | *n/a*
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [update_attribute](http://apidock.com/rails/ActiveRecord/Persistence/update_attribute)| Yes | Yes | No | Yes | Yes | Yes
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [update](http://apidock.com/rails/ActiveRecord/Persistence/update)| Yes | Yes | Yes | Yes | Yes | Yes
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [update_column](http://apidock.com/rails/ActiveRecord/Persistence/update_column)| No | Yes | No | No | No | Yes
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [update_columns](http://apidock.com/rails/ActiveRecord/Persistence/update_columns)| No | Yes | No | No | No | Yes
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [User::update](http://apidock.com/rails/ActiveRecord/Relation/update)| Yes | Yes | Yes | Yes | Yes | Yes
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [User::update_all](http://apidock.com/rails/v4.0.2/ActiveRecord/Relation/update_all)| No | Yes | No | No | No | No
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------

## Rails 3

  Method | User default accessor | Saved to DB | Validitions | Mass-Assignment protection
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [attribute=](http://apidock.com/rails/ActiveRecord/AttributeMethods/Write/attribute)| Yes | No | *n/a* |  No
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [attributes=](http://apidock.com/rails/ActiveRecord/AttributeAssignment/attributes%3D)| Yes | No | No | Yes
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [write_attribute](http://apidock.com/rails/ActiveRecord/AttributeMethods/Write/write_attribute)| No | No | *n/a* | No
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [update_attribute](http://apidock.com/rails/ActiveRecord/Persistence/update_attribute)| Yes | Yes | No | No
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
  [update_attributes](http://apidock.com/rails/ActiveRecord/Persistence/update_attribute)| Yes | Yes | Yes | Yes
  ------ | --------------------- | ----------- | ----------- | --------- | ------------------ | --------------
