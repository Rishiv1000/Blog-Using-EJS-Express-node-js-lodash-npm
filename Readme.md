
![Screenshot 2023-12-31 230733](https://github.com/Rishiv1000/7.Blog-Using-EJS-Express-node-js-lodash-npm/assets/114014651/b9b46f17-9439-47b5-84bc-da589f7a777f)


check dependencies -
if  node module is not downloaded  then type "npm install"
 
first ADD INTO JS FILE (app.js) for load modules .........  const express = require("express");
                                                   const bodyParser = require("body-parser");
                                                   const ejs = require("ejs");
                                                   var _ = require('lodash');

Also add their text ............................... const app = express();
                                                    app.set('view engine', 'ejs'); ..................................... this is use for ejs file in view folder
                                                    app.use(bodyParser.urlencoded({extended: true}));
                                                    app.use(express.static("public")); ................................. it is for public our css file



C1 - load one ejs/html (Home.ejs) file in public folder into browser server frontend using app.get. but home.js contains only "<h1>HOME</h1>" file
       
       app.get('/',function(req,res){
                        res.render("home") 
       })

C2 - const homecontent = "jnkmas633" which is present in js file as a constant ,show in front of browser as a paragraph.
     
     ADD INTO HOME.EJS... <h1>WELCOME TO HOME</h1>
                          <p1>  <%= First %> </p1>

     ADD INTO APP.JS..... app.get('/',function(req,res){
                              res.render("home", {First : homecontent}) 
                          })



C3 - add another header html and footer html which is present in another ejs file (header.ejs & footer.ejs) into home.ejs.
   
     ADD INTO HOME>EJS.... <%- include('header.ejs') -%>
                           <h1>WELCOME TO HOME</h1>
                           <p1>  <%= First %> </p1>
                           <%- include('footer.ejs') -%>``

C4 - put header and footer files into partials folder in view folder... and change {include('partials/header.ejs')} on C3.

C5 - its make working options in frontend ( About && Contact )..

    first ,add html into about.ejs... <%- include("partials/header.ejs") -%>                               
                                <h1> ABOUT</h1>
                                <p> <%= abouttext %> </p>
                                <%- include("partials/footer.ejs") -%>

          and  contact.ejs... <%- include("partials/header.ejs") -%>
                             <h1> CONTACT</h1>
                            <p> <%= contacttext %> </p>
                            <%- include("partials/footer.ejs") 
  
    Secondly , make extra app.get for about and contact..... app.get('/about',function(req,res){
                                                               res.render("about", {abouttext : aboutContent})
                                                            })

                                                            app.get('/contact',function(req,res){
                                                               res.render("contact", {contacttext : contactContent})
                                                            })
                                                                        
    third, change the anchor tag href into " href= "/about" and "/contact" in header.ejs file 

C6 - make working link of "/compose"
 
     add into compose.ejs ..... <%- include("partials/header.ejs") -%>
                              <br>
                            <h1>Publish Your New Blog</h1>
                           <input type = "text" name= "" >
                           <button type = "submit" name= "button"> PUBLISH </button>
                            <%- include("partials/footer.ejs") -%>

    make app.get for compose ......  app.post('/compose',function(req,res){
                                                     res.redirect('compose')
                                      })

C7 - using bootstrap make a title bar && post bar into compose.ejs &&&  take inputs from compose into terminal as a JSON .

       first , in C6 take input buttons of title and post in a <form> tag ................ <form action="/compose" method = "post">                                       
                                                                                             <input type="text" class="form-control" id="exampleFormControlInput1" name="">
                                                                                              <input type="text" class="form-control" id="exampleFormControlInput1" name="">
                                                                                              <button class="btn btn-primary" type="submit" value="Submit">
                                                                                           </form>

        secondly , for take input form body parser we give them name ....   <input type="text" class="form-control" id="exampleFormControlInput1" name="dataoftitle">
                                                                             <input type="text" class="form-control" id="exampleFormControlInput1" name="dataofpost">
       
       third, we type into js file .........  app.post('/compose',function(req,res){
                                                     var dataobj =  {
                                                               title : req.body.dataoftitle ,
                                                               data : req.body.dataofpost
                                                     }
                                                     res.redirect('/') ................................................ its is use to redirect into home page
                                              })  

C8 - we have to try taking data from compose and add into home.ejs.

     to do this we ejs to sent data as a array ... first, we make a posts array 
                                                                              let blogs = []

     Secondly, take data from body parser and push into array 
                                                  app.post('/compose',function(req,res){
                                                                            var dataobj =  {
                                                                                    title : req.body.dataoftitle ,
                                                                                    data : req.body.dataofpost
                                                                            }
                                                                            blogs.push(dataobj)
                                                                            res.redirect('/')
                                                   })

     Third, pass into home.ejs as a array.
                               to do this we add some html into home.ejs ........ app.get('/',function(req,res){

                                                                                       res.render("home", {First : homeStartingContent, array : blogs}) 
                                                                                  })

                                 add JS AND EJS in home.ejs for each input data ................ <% array.forEach( function(element){ %>
                                                                                                   <h1> <%= element.title %> </h1>
                                                                                                   <p> <%= element.data %>
                                                                                                      
                                                                                               <% })  %>

C9 - Using Express Route , we want to typed address input (localhost:3000/posts/....) for particular post and take into terminal 
                                                 
                                  app.get("/posts/:topic", (req,res)={
                                                          console.log(req.params.topic);
                                 )}

C10 -  check the type post addess title is same from all stored posts.

                  app.get("/posts/:topic", (req,res)={
                           var linktopic = req.params.topic);  
 
                           blogs.forEach(function(element ){
                                       var title = element.title
                                       if(linktopic == title){ 
                                             console.log("match found")
                                         }
                         }
                }

C11 - solve the problem of lower case or upper case typing by using lodash.

        first , install lodash......
        secondly, add into app.js ........ var _ = require('lodash');
        
        third ,  make both lowercase - "typed postname" and "stored postname" into above code  , then compare them
                                                                                             const linktopic = _.lowerCase(req.params.topic)
 
                                                                                            blogs.forEach(function(element ){
                                                                                                        var title =_.lowerCase(element.title)
    

C12 -  add html into post.ejs.
                                  <%- include("partials/header.ejs") -%>
                                   <h1> <%= topic  %> </h1>
                                   <p> <%= content  %> </p>
                                  <%- include("partials/footer.ejs") -%>

C13 - add typed post data into post.ejs. 
                                    app.get("/posts/:topic", (req,res)=>{

                                           const linktopic = _.lowerCase(req.params.topic)
 
                                           blogs.forEach(function(element ){
                                                          var title =_.lowerCase(element.title)
    
                                          if(linktopic == title){ 
                                                   res.render("post", {topic : element.title ,content : element.data})
                                          };
  
                                   })})

C14 - make home page some clear .
          to do this , we show post title and some posts data within 100 characters .

                                   <% array.forEach( function(element){ %>
                                                      <h1> <%= element.title %> </h1>
                                                       <p> <%= element.data.substring(0,100) + "...."  %>
                                                      <a href=""> Read More</a></p>    
                                  <% })  %>

C15 - make clickabe "READ MORE" Into home pages 
        
      to do this , we add href link into above code.
                                  
                                 <a href="/posts/<%= element.title %>"> Read More</a></p>    


      
    

       
                                                   


    
 
    
