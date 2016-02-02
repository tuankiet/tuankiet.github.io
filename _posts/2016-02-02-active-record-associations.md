---
layout: post
title:  "Active record associations"
date:   2016-02-02
categories: ruby
---

> Review active record query

Types of association?

There are 6 types of association: `belongs_to`, `has_one`, `has_many`, `has_many :through`, `has_one :through`, `has_and_belongs_to_many`.

`belongs_to` must use in singular term, it describe one-to-one association. Example: `1 Order` belongs to `1 Customer`
in Order's model has field name `customer_id` (int), and `customer_id` referer with id of Customer's model.

`has_one` built for one-to-one association, it set-up connection between 2 models, but it has somewhat different semantics.
Example each Supplier has one Account, in Account's model has field name `supplier_id`(int).

`belongs_to` and `has_one` is same when we write migrations, we need to indexing the column connection (`customer_id`, `supplier_id`)

`has_many` is indicates one-to-many associations. Example: `1 Customers` has many `n orders`. Instance of Customer's model has many instance of orders's model.
Example: Customer has many orders. in Order's model has field `customer_id`(int)

Example migration:

{% highlight ruby %}
class CreateOrders < ActiveRecord::Migration
  def change
    create_table :customers do |t|
      t.string :name
      t.timestamps null: false
    end

    create_table :orders do |t|
      t.belongs_to :customer, index: true
      t.datetime :order_date
      t.timestamps null: false
    end
  end
end
{% endhighlight %}

The `has_many :through` association. it often use to set up many-to-many connection. It indicates One model can be matched with many instance other model `through` the third model.
Example: consider context about medical practice, where patients make appointments to see physicians. 1 Physician can has many patients through appointments. Physician has many patients and has many appointments. Then, appointments belongs to physician, and belongs to appointment. Like physicians, 1 Patient can has many physicians through appointments. Patient has many physicians, has many appointments.
Appointments model has 2 field bo describe the connection is `physician_id` and `patient_id`.

Example:

{% highlight ruby %}
class Physician < ActiveRecord::Base
  has_many :appointments
  has_many :patients, through: :appointments
end
 
class Appointment < ActiveRecord::Base
  belongs_to :physician
  belongs_to :patient
end
 
class Patient < ActiveRecord::Base
  has_many :appointments
  has_many :physicians, through: :appointments
end
{% endhighlight %}

Sometime The `has_many :through` use like `shortcuts`, example we have Document, the document has many sections, one session has many paragraphs. So we can get paragraphs of ducuments by Document.paragraphs by this association.

{% highlight ruby%}
class Document < ActiveRecord::Base
  has_many :sections
  has_many :paragraphs, through: :sections
end
 
class Section < ActiveRecord::Base
  belongs_to :document
  has_many :paragraphs
end
 
class Paragraph < ActiveRecord::Base
  belongs_to :section
end
{% endhighlight %}

The `has_one :through` association. Like `has_many :through`, it indicates connection through the third model, but it has one connection.
