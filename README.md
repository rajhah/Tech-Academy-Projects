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
+ [BlogAuthor Part 2- Details and Delete Page Styling --------15955](#blogauthor-part-2--details-and-delete-page-styling---------15955)
+ [BlogAuthor Part 3- Index Page Styling --------15972](#blogauthor-part-3--index-page-styling---------15972)
+ [BlogAuthor Part 4- Async Delete --------15980](#blogauthor-part-4--async-delete---------15980)
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

### BlogAuthor Part 2- Details and Delete Page Styling --------15955
### BlogAuthor Part 3- Index Page Styling --------15972
### BlogAuthor Part 4- Async Delete --------15980
### HeadAuthor Part 1- Create User --------15981
### HeadAuthor Part 2- Seed HeadAuthor user in database --------15982
### HeadAuthor Part 3- Restrict CRUD operations --------16017
### HeadAuthor Easy Login Button --------16018
### Link BlogAuthor and BlogPost --------16030
