# Tech-Academy-Projects
This repository was initially to be used for Tech Academy Projects.
Eventually all the projects ended up in other repos, so instead here you can find the summary of my time on the Tech Adademy Live Project

## Tech Academy Live Project
The Live Project took place from Jan 23rd to Feb 3rd, 2023. It was a two week project designed to simulate a typical development sprint with daily standups and weekly retrospective meeetings. The project was ongoing work on a web application. Below is a list of the tickets I worked on during this time, along with descriptions and code snippets as appropriate.

We used .NET Framework 4.7.2 for this project

Most of my code was written in C#, but it also included some javascript and css.

+ [Count Developers on Project page](#count-developers-on-project-page)
+ [Create BlogAuthor Table](#create-blogauthor-table)
+ [BlogAuthor Part 1- Create and Edit Page Styling](#blogauthor-part-1--create-and-edit-page-styling)
+ [BlogAuthor Part 2- Details and Delete Page Styling](#blogauthor-part-2--details-and-delete-page-styling)
+ [BlogAuthor Part 3- Index Page Styling](#blogauthor-part-3--index-page-styling)
+ [BlogAuthor Part 4- Async Delete](#blogauthor-part-4--async-delete)
+ [HeadAuthor Part 1- Create User --------15981](#headauthor-part-1--create-user---------15981)
+ [HeadAuthor Part 2- Seed HeadAuthor user in database --------15982](#headauthor-part-2--seed-headauthor-user-in-database---------15982)
+ [HeadAuthor Part 3- Restrict CRUD operations --------16017](#headauthor-part-3--restrict-crud-operations---------16017)
+ [HeadAuthor Easy Login Button --------16018](#headauthor-easy-login-button---------16018)
+ [Link BlogAuthor and BlogPost --------16030](#link-blogauthor-and-blogpost---------16030)

### Count Developers on Project page
This ticket was a "get your feet wet" story ticket that involved counting the number of developers in a list and displaying that number at the top of the page. I used javascript for this. Each developer was assigned to create their own paragraph using a new \<p\> tag, so this code counts all such tags on the page and changes the value of a bootstrap badge with the id "\#NumPersons" to display the number when the page loads.

Here you can see the complete code for this story:

<img width = "415" alt="image" src="https://user-images.githubusercontent.com/109645238/220472145-8180c82f-00c4-46e2-ad65-75215ac09b62.png">

### Create BlogAuthor Table
For this story ticket I was tasked with creating a new table to hold information about different blog authors on the website. To do this I created a new model class for the database table, because the solution was using a code-first model.

Here is the class model I created:

<img width="415" alt="image" src="https://user-images.githubusercontent.com/109645238/220474209-1d829e0b-a923-4bfd-b29a-f35c54559277.png">

I also added a DbSet to our application's DbContext class:

<img width = "415" alt="image" src="https://user-images.githubusercontent.com/109645238/220474752-e9376e4c-75f0-44d6-a33f-9ed1d8bdd684.png">

Creating this model and updating the database automatically scaffolded the necessary pages on the web app for CRUD operations on the table. The next few tickets were concerned with styling these pages and limiting access to certain functions according to a user's priviliges on the website.

### BlogAuthor Part 1- Create and Edit Page Styling
This was the first ticket that represented a significant work effort. The requirements were to style the Create and Edit pages that had been generated automatically to match the look and function of a designed prototype.

Notably, the "Bio" entry was required to be formatted as a TextArea, and the "Joined" and "Left" entries had to be changed to date pickers, rather than simple input fields.

.NET automatically interpretes the data annotation "[DataType(DataType.MultilineText)]" as a textarea when rendering the view, so in order to do that I simply added this annotation in the BlogAuthor model class, as well as a little css to display it more clearly to the user.

<img width = "415" alt="image" src="https://user-images.githubusercontent.com/109645238/220477439-04c5b12d-34ed-41d9-9d89-ca3589df8209.png">

<img width = "415" alt="image" src="https://user-images.githubusercontent.com/109645238/220484624-20eaa9aa-8b2e-439f-9f22-dcc70f51c40f.png">


In order to set the Joined and Left fields to be date pickers, I set the type to datetime-local. You can see this in the included code below. I'm providing the full code this time to give a sense of the whole page. Subsequent pages will not be included, but I feel it was necessary to explain a few things. 

First, most of the classes inside the html tags are either bootstrap classes or custom classes created by me to align with the design prototype.

Second, you can clearly see the layout of the form created by .NET when the page was scaffolded. In this example I made only a few small changes to the stylings of the buttons - the action methods remained unchanged. In future tickets, you will see some examples that needed to be modified to work correctly.

Here is what the completed Create page looks like, or at least, the section that I worked on:

<img width = "415" alt="image" src="https://user-images.githubusercontent.com/109645238/220484109-1e4a7ea1-ce79-4979-a254-43c4ae20b04c.png">

The Edit page is similar, but with the information pre-filled from the database for whichever user is selected.

<details>
  <summary>Create Page cshtml</summary>

    @using (Html.BeginForm()) 
      {
          @Html.AntiForgeryToken()

          <div class="form-horizontal cms-bg-main-light BlogAuthor-CreateEdit--form">
              @*<h4>BlogAuthor</h4>
                  <hr />*@
              @Html.ValidationSummary(true, "", new { @class = "text-danger" })
              <div class="form-group">
                  @Html.LabelFor(model => model.Name, htmlAttributes: new { @class = "control-label col-md-2" })
                  <div class="col-md-12">
                      @Html.EditorFor(model => model.Name, new { htmlAttributes = new { @class = "form-control" } })
                      @Html.ValidationMessageFor(model => model.Name, "", new { @class = "text-danger" })
                  </div>
              </div>

              <div class="form-group BlogAuthor-CreateEdit--textarea">
                  @Html.LabelFor(model => model.Bio, htmlAttributes: new { @class = "control-label col-md-2" })
                  <div class="col-md-12">
                      @Html.EditorFor(model => model.Bio, new { htmlAttributes = new { @class = "form-control" } })
                      @Html.ValidationMessageFor(model => model.Bio, "", new { @class = "text-danger" })
                  </div>
              </div>

              <div class="d-flex justify-content-center">
                  <div class="form-group">
                      @Html.LabelFor(model => model.Joined, htmlAttributes: new { @class = "control-label col-md-2" })
                      <div class="col-md-10">
                          @Html.TextBoxFor(model => model.Joined, new { @type = "datetime-local", @Value = DateTime.Now.ToString("yyyy-MM-ddThh:mm") })
                          @*@Html.ValidationMessageFor(model => model.Joined, "", new { @class = "text-danger" })*@
                      </div>
                      <div class="cms-bg-dark text-center my-2 ml-3 col-md-10">
                          @Html.ValidationMessageFor(model => model.Joined, "", new { @class = "text-danger" })
                      </div>
                  </div>

                  <div class="form-group">
                      @Html.LabelFor(model => model.Left, htmlAttributes: new { @class = "control-label col-md-2" })
                      <div class="col-md-10">
                          @Html.TextBoxFor(model => model.Left, new { @type = "datetime-local" })
                          @Html.ValidationMessageFor(model => model.Left, "", new { @class = "text-danger" })
                      </div>
                  </div>
              </div>

              <div class="form-group col-md-offset-2 col-md-10 d-inline-block">
                  <div class="d-inline-block">
                      <a href='@Url.Action("Index")' class="btn btn-dark">Back to List</a>
                  </div>
                  <div class="d-inline-block">
                      <input type="submit" value="Create" class="btn btn-default cms-bg-secondary cms-text-light" />
                  </div>
              </div>
          </div>
      }

</details>

### BlogAuthor Part 2- Details and Delete Page Styling
The purpose of this ticket was to add styling to the Blog Author Details page, remove the delete page, and move the delete action onto a button in the Details page. The final product looked like this:

<img width = "415" alt="image" src="https://user-images.githubusercontent.com/109645238/220517989-89c3d67d-bd01-4ca3-b460-49e977d0a54f.png"/>

The "Author Pages" and "Blog Posts" buttons are part of a navbar that toggle between the information page for the author and a dummy page that will eventually implement a list of blog posts. Rather than writing any JS for this, I used built-in bootstrap classes made for this purpose. No reason to re-invent the wheel!

The "data-toggle" and "href" attributes work together to select the correct information to display via the id further down the page.

<img width = "415" alt="image" src="https://github.com/rajhah/Tech-Academy-Projects/blob/main/navbar.png"/>

The social media buttons on this page are dummy links until actual author information is connected.

The Delete button will delete the current author and redirect the user to the Index page.

### BlogAuthor Part 3- Index Page Styling
For this ticket I created a partial view based on the previous ticket for the details page. This partial view rendered an author details card for each entry in the model. All the content on the index page was simply replaced by this partial view. A few stylistic changes were made as well.

### BlogAuthor Part 4- Async Delete
This ticket was for creating an asyncronous method to delete author records. There were several steps required for this:
* Front end modal and functionality
* Ajax call triggered by delete button click
* New Async method in the controller

I handled the front end, again, mostly with built-in bootstrap classes, Unsurprisingly, this turned out to be the easy part:

<img width = "415" alt="image" src="https://user-images.githubusercontent.com/109645238/221034388-0cb39c5c-73b3-4963-95ba-799a739c864e.png" />

<details>
  <Summary>Modal Code</Summary>
     
      <div class="modal fade" id="deleteModal-@item.BlogAuthorId" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
          <div class="modal-dialog" role="document">
              <div class="modal-content cms-bg-light cms-text-dark">
                  <div class="modal-header">
                      <h5 class="modal-title" id="deleteModalLabel">Confirm Delete?</h5>
                      <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                          <span aria-hidden="true">&times;</span>
                      </button>
                  </div>
                  <div class="modal-body">
                      Are you sure you want to delete author @item.Name?
                  </div>
                  <div class="modal-footer">
                      <button type="button" class="btn btn-dark" data-dismiss="modal">Cancel</button>
                      @*When delete button is clicked, call an async method to delete THIS BlogAuthor*@
                      <form>
                          @Html.AntiForgeryToken()
                          <input type="hidden" id="BlogAuthorId" value="@item.BlogAuthorId" />
                          <button type="button" class="BlogAuthor-Index--DeleteAsync btn cms-bg-main cms-text-light" data-dismiss="modal">Delete</button>
                      </form>
                  </div>
              </div>
          </div>
      </div>
</details>

In the js file I created an ajax call to the new controller method that triggered when the button was clicked. Using the .closest("form") tag ensured that the correct id was sent through to be deleted. If a successful response is returned from the method, the rest of the code animated the deletion of the actual html element so that it looked pleasing to the user.

<details>
  <summary>js Code</summary>

    $(".BlogAuthor-Index--DeleteAsync").click(function () {
      var form = $(this).closest("form");
      var authorId = form.find("#BlogAuthorId").val();
      var token = form.find("input[name='__RequestVerificationToken']").val();
      $.ajax({
          type: "POST",
          url: "/Blog/BlogAuthors/AsyncDelete",
          data: {
              id: authorId,
              __RequestVerificationToken: token
          },
          success: function (data) {
              if (data.status == "success") {
                  //Item was successfully deleted

                  //Animation:
                  /*
                   *Using jQuery, animate the opacity of the card to 0 over 1 second
                   *Then use the callback function to squash the height so everything else slides up
                   *Finally, use the nested callback to hide the element completely when the animation is done
                   */
                  $("#BlogAuthorCard-" + authorId).animate({ opacity: 0 }, 1000,
                      function () {
                          $(this).animate({ height: 0 }, 165,
                              function () {
                                  $(this).hide();
                              });
                      });
              } else {
                  //Send an error message if author id not found in db
                  alert("Error: Author does not exist.")
              }
          },
          error: function () {
              alert("Error: Author could not be deleted.")
          }
      });
    });
</details>

<details>
  <summary>Controller Code</summary>
  
        // POST: Blog/BlogAuthors/AsyncDelete
        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> AsyncDelete(int id)
        {
            BlogAuthor blogAuthor = await db.BlogAuthors.FindAsync(id);
            if(blogAuthor != null)
            {
                db.BlogAuthors.Remove(blogAuthor);
                await db.SaveChangesAsync();
                return Json(new { status="success", authorId = id });
            }
            return Json(new { status = "error" });
        }
  
</details>


### HeadAuthor Part 1- Create User --------15981
### HeadAuthor Part 2- Seed HeadAuthor user in database --------15982
### HeadAuthor Part 3- Restrict CRUD operations --------16017
### HeadAuthor Easy Login Button --------16018
### Link BlogAuthor and BlogPost --------16030
