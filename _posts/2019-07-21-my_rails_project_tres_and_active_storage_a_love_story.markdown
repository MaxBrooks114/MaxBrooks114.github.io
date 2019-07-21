---
layout: post
title:      "My Rails Project Tres & Active Storage: a Love Story. "
date:       2019-07-21 17:35:46 +0000
permalink:  my_rails_project_tres_and_active_storage_a_love_story
---


This project was by far, the most involved, invigorating, and infuriating project of the bootcamp. I learned way more than I expected to learn (was definitely a victim of scope creep), and I can safely say I am a rails...novice. Truly this framework is so vast I feel like I have barely cracked the surface of what it can do. I did not want to do the bare minimum so I decided I wanted to make something that a) I would be passionate about, and b) people might want to use.

My project, SongBook, is a rails app designed to give the use the ability to maintain their library of songs that they as a musician know how to play, want to learn, or are writing themselves. As I was talking about it with my co-worker (who's kind of a douche) remarked: "Okay...but why can't I do this all with a spreadsheet?". This sent me reeling. I guess you could. But it also got me thinking okay- what can't a spreadsheet do? In terms of this app, you would not be able to upload and listen to music files! 

At this point I did some searching and found one of Rails' coolest, and newest features- Active Storage! Active storage lets you upload files to your app and stores them on your local drive for manipulation and/or display! How cool?! After some finageling I got it all working, and for the rest of this post I will show you how I used it. 

First in your terminal type `rails active_storage:install` this will create migration files for you (you will need Rails 5.2 or above):

`# This migration comes from active_storage (originally 20170806125915)
class CreateActiveStorageTables < ActiveRecord::Migration[5.2]
  def change
    create_table :active_storage_blobs do |t|
      t.string   :key,        null: false
      t.string   :filename,   null: false
      t.string   :content_type
      t.text     :metadata
      t.bigint   :byte_size,  null: false
      t.string   :checksum,   null: false
      t.datetime :created_at, null: false

      t.index [ :key ], unique: true
    end

    create_table :active_storage_attachments do |t|
      t.string     :name,     null: false
      t.references :record,   null: false, polymorphic: true, index: false
      t.references :blob,     null: false

      t.datetime :created_at, null: false

      t.index [ :record_type, :record_id, :name, :blob_id ], name: "index_active_storage_attachments_uniqueness", unique: true
      t.foreign_key :active_storage_blobs, column: :blob_id
    end
  end
end`

Run the migration! Then in the models you want to upload files for add the code: `has_one_attached :(name_of_attachment)` or `has_many_attached :(name_of_attachment)` depending on how many attachments you want each instance of the model you'll want to...attach. This will unlock the method `attached?` which returns a boolean depending on whether you have the file attached to the object or not. This will come in handy for display, removal and validations. 

But Max! How do we actually upload the files?!! I'll tell ya. 

In your form just use the helper `file_field` just like you would `text_field` or anything other field. This will prompt your files finder window opening and you could attach a file like you would in most other.  In your view just place the appropriate tag for the file to show it. In my case it was `<%= audio_tag(url_for(@object.attachment)) %>`. You may not need the `url_for` depending on the file type and what you want to do with it.  You also may need to nest it in an if statement that checks for the attachment in the first place. You can do this using..you guessed it! `attached?`. 

Removing attachments is a tad more complicated, and validations don't exist yet (you'll have to write custom ones). Maybe I will do another blog post. Tootles! 





