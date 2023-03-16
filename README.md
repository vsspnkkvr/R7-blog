# README

## Code The Dream Rails Lesson 3.1

This repository contains the framework for the blog application you will develop as you go through the Treehouse videos on Rails development. You should create a Git branch called `blog` and make your changes to that branch. You should add and commit your changes periodically, and you should push them to GitHub periodically as well. Then, on completion of the Treehouse Rails videos, once you have made all the changes to your version of the blog application that the instructor in the videos has made, you can submit a pull request for review.

After you clone your assignment repository, be sure to run `bundle install`. That will be necessary for subsequent Rails assignments as well.

As you watch the videos, be sure you make all the changes that the instructor is making to his version of the application. Otherwise you won't remember the video content. At several times during the videos, the instructor shows alternate ways of doing things. You should comment out what you have instead of erasing it, so that you have a record of each way to implement a particular function.

Because the Treehouse videos use an old version of Rails, there are differences between the code in the videos and the code that you will be working with. Some of these differences are large enough that you will need to do the following as you are watching them:

* In the "Our First Resource" video of the "Ruby on Rails 5 Basics" section, immediately _after_ you enter the command:

  ```bash
  $ bin/rails generate scaffold Post title:string
  ```

  You must then also enter the following command:

  ```bash
  $ cp rails5/* app/views/posts
  ```

  Otherwise, it will be more difficult to follow along their videos later. If the command comes back with the error `cp: app/views/posts is not a directory`, then that means the `scaffold` command failed or was not yet run.
* In the "Finding a Page" video of the "Rails Routes and Resources" section, after the instructor types in `render text` in the `show` method, you must instead type `render plain`. Otherwise, you will get a "Missing template" error.
* At the end of the "Route to Create Pages" video of the "Rails Routes and Resources" section, do not be alarmed if you're unable to cause the error "The action 'create' could not be found for PagesController"; Rails 7 simply hides this under the hood and resets the form, so ignore this and continue to the next video.
* In the "Controller Action to Create Pages" video of the "Rails Routes and Resources" section, at the parts where the instructor enters `render text: params.to_json` into the `create` method or modifies the value after `text:`, skip doing this on your machine, and keep following along the video as if what appears in the instructor's browser appeared on yours. Rails 7 handles form submissions differently, so you won't be able to see the plain text responses without browser tools.
  * If you are familiar with using browser tools to inspect asynchronous requests, you may choose to replace `render text` with `render plain` (just like you did in the "Finding a Page" section) and inspect the plain text response in the Network tab.
* At the end of the "An Edit Form" video of the "Rails Routes and Resources" section, Rails 7 is again hiding the error and resetting the form. Continue to the next video.
* In the "Deleting Pages" video of the "Rails Routes and Resources" section, when you add the Delete link to the `show` view, paste this in instead:

  ```erb
  <%= link_to 'Delete', @page, data: { turbo_method: :delete, turbo_confirm: "Are you sure?" } %>
  ```

When you complete Rails Routes and Resources, push your `blog` branch to GitHub, create a pull request, and link it on the assignment submission form that's found on the class page.

## Code The Dream Rails Lesson 3.2

For assignment 3.2, you will continue work with this same repository. While the blog branch is active, create a new branch called associations.  This involves work to be added to the work in the blog branch.  You will use the same repository, but the new associations branch, to complete the work in the video called Active Record Associations in Rails.

As with the previous lesson, there are some differences in the Treehouse videos that you will have to look out for:

* In the "Has Many Associations" video, there is no error when you first enter `post.comments` in the Rails console. Just ignore this and continue.
* At the beginning of the "Belongs to Associations" video, entering `comment = Comment.last` alone does not display the comment's attributes, so you will have to enter `comment` as an additional command in order to see its `post_id`.
* In the "Active Record Associations in Rails" video, after you run the command:

  ```bash
  $ bin/rails generate migration AddPostToComments post:references
  ```

  If you run `bin/rails db:migrate` while there are still comments in the database, the migration will fail with output that starts similarly to:

  ```
  == 20240503123456 AddPostToComments: migrating ================================
  -- add_reference(:comments, :post, {:null=>false, :foreign_key=>true})
  rails aborted!
  StandardError: An error has occurred, this and all later migrations canceled:

  SQLite3::ConstraintException: NOT NULL constraint failed: comments.post_id
  /Users/student/repos/R7-blog/db/migrate/20240503123456_add_post_to_comments.rb:3:in `change'
  ```

  This is because Rails 7's `references` migrations are stricter by default and include `null: false` on foreign key columns. In our case, when the database attempts to adds the `post_id` column to the `comments` table, it does not know what `post_id` value to give to existing comments in the table, and because the value cannot be `null`, it results in an error.

  While it is possible to roll back, delete the `null: false` from the migration and re-run `db:migrate`, we recommend that you instead go into the Rails console and enter `Comment.destroy_all` because it is generally better to avoid nullable foreign key columns if possible.
* Later in the same video, when you run the command:

  ```bash
  $ bin/rails g model Comment content:text name:string post:references
  ```

  Rails 7 will not ask whether to overwrite anything; you must instead add the `--force` flag to the end of the command.
* In the "Updating an Associated Record" video, when the instructor submits the comment edit form and sees the message "The action 'update' could not be found for CommentsController", Rails 7 is again hiding the error and resetting the form, like you have already seen in the previous week. Just continue with the video.
* In the "Deleting an Associated Record" video, when you add the Delete link to the `show` view, paste this in instead:

  ```erb
  <%= link_to "Delete", [@post, comment], data: { 'turbo-method': :delete } %>
  ```

  Like in the previous video, you will not see an error message when you click the Delete link immediately after adding it to the view, so continue with the video.

You are done with it when you have completed that last video. The Active Record Associations in Rails video also involves several other Rails applications, and there are separate assignments and git repositories for those.  They are the community, periodical, and mdb repositories.  You should submit one assignment with links to all four pull requests.
