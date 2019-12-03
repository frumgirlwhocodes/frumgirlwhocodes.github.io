---
layout: post
title:      "Deleting a resource using Javascript and Jquery"
date:       2019-12-03 20:15:53 +0000
permalink:  deleting_a_resource_using_javascript_and_jquery
---


I created a Recipe App using Ruby on Rails backend and Javascript front end. I used Javascript and Jquery to delete a recipe instead of just the rails backend. In my list of recipes aka the index page, I wrote a link using a HTL tag as such;       <a href= "#" class="js-delete" data-id= "<%= recipe.id %>"> Delete </a>.  when you click on this link it should delete the recipe. 

How?

With Javascript code. On the link i called a class "js-delete and I used Javascript and Jquery as such: 

function clickDelete() {
    $(".js-delete").on('click', function(event) {
      event.preventDefault() 
      let id = $(this).attr("data-id");
 fetch("/recipes/" + id, {method: 'DELETE'}).then(() => {
          console.log('removed');
       }).catch(err => {
         console.error(err)
       });
    }) 
  }
	
	I used jquery on the  class "js-delete, then  when the link is clicked it will fire up a click event and prevent the default of the event. I then used a fetch request to get the route of the recipe I wanted to delete which required you to get a id, so I did this by getting the attribute of the data-id which I put in my HTMK href tag.  
	then after the fetch request deletes it, it should say removed in the console showing that the request worked. 
	
	make sure to add, protect_from_forgery with: :null_session, in the application controller and skip_before_action :verify_authenticity_token, except: [ :destroy] in the controller that has the delte action, or else you may have some erros with the authenticity when you want to delete a resource.  
	
you might also have problems with the server. Try to refresh the page and the item will be removed and no longer be shown in your list of recipes 


