---
layout: post
title: "Refactor Model with Concerns, Services, Presenters"
date: 2016-02-17
categories: ruby
---

> Keep healthy and reduce responsibility of ActiveRecord models

# Presenters

Presenters pattern may be more commonly known as the View-Model pattern.

The idea is we want to seperate the code related with presenting and displaying attribute of instance model in View. View != Model. The Model only contains logic code, don't need to contain code related with View.

We call it a `Presenter`, it's just a class represent displaying logic from Model to View.
Presenters or Decorators can solve problem:
- too much logic code in Views
- large size helpers
- models has view code

