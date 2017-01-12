**Code-Challenge**
================

OVERVIEW
--------------------
This code challenge utilizes Docker to install the basics of our environment. We have created a module for our Prestashop site and have included it here as a submodule. The module can be found [here](https://github.com/davidvarney/code-challenge-module). This was done this way because we wanted to separate our dev environment from the module. Typically a module can be added to Prestashop in several different ways. By having our module be a standalone project it allows us to have the option of either downloading the module via a Zip file and uploading it manually into Prestashop or we can simply add it to Prestashop as a submodule.

Steps for Setup/Install
--------------------------------

 1. Clone this Repository into your local dev machine
	 - `git clone https://github.com/davidvarney/code-challenge`
 2. Download and install Docker for your local dev machine by going [here](https://www.docker.com/products/docker)
 3. Next, we want to use the Prestashop Docker image that is released and maintained by the Prestashop team. So, we should next run the following command from a command prompt/terminal: 
 `docker run -ti --name prestashop-container-name -v YOUR/LOCAL/modules/DIRECTORY/HERE:/var/www/html/modules -p 8080:80 -p 3306:3306 -d prestashop/prestashop:1.6.1.9`
 NOTE: After researching and learning how to use Docker it became clear to me that the DB should be segregated in it's own container. Since this is just an example exercise I didn't bother setting up a Dockerfile and then a Docker Compose file to bring both the Prestashop image and the DB image to create the two separate containers. 
 4. As part of the stipulations of the Code Challenge we only want to run Prestashop v 1.6 so 1.6.1.9 was the last release for 1.6. Also, we can map the "override" and "themes" directories as well by specifying that in our Docker Run command. For my environment I mapped the `modules` directory inside of the Prestashop container to the following one my dev machine: `-v ~/Desktop/Development/code-challenge/modules:/var/www/html/modules`
 5. Once you have Docker up and running the Prestashop image you can make your way to http://localhost:8080 in your browser and go through the install setup of Prestashop. NOTE: DB password should just be 'admin' without the quotes.. We can double-check the environment settings by looking at the Prestashop 1.6.1.9 Dockerfile [here](https://github.com/PrestaShop/docker/blob/master/images/1.6.1.9/Dockerfile). You can also feel free to change those localhost port numbers to whatever you would like.
 6. After installing Prestashop and renaming the install/admin directories we can make our way to the Admin area and go to the "Modules and Services" section.
 7. You shouldn't see our module within the list of modules in the Admin area just yet. Since our Prestashop module is in this project as a submodule we need to pull it into our freshly cloned Code-Challenge project. From the Code-Challenge root directory run the following:
`git submodule update --init`
 8. Next, refresh the Module and Services page  and search for our module which should be named "Code-Challenge-Module". HINT: It should be a bit quicker to just search within the "Other Modules" section.
 9. Click "install" and then click the "Proceed with the Installation" in the modal that pops up.
 10. Next, put in the zip code of where you would like to view weather from and click the "Save" button.
 11. We want to easily view this module on the front end so make your way over to the "Modules and Services" sub-menu "Positions".
 12. Once you're in the "Positions" section lets make sure to search for the "Code-Challenge-Module".
 13. I created two hooks for this module, "displayLeftColumn" and "displayRightColumn". Lets go ahead and move the "Code-Challenge-Module" into position 1 under the "displayLeftColumn".
 14. We can make our way to the front-end of the Prestashop site by going to localhost:8080
 15. You can click on any of the following categories "Women", "Dresses", or "T-Shirts".
 16. Finally, once you've clicked on one of those categories you will see that we have our module on the top of the left-side column.

Production Server Setup
----------------------------------
The production server that I deployed this to is a server running over at Digital Ocean. I went ahead and created one of the smaller instances with an Ubuntu 16.04 and Docker image. I went ahead and pointed my domain (davidvrney.com) to the production server so I wouldn't have to remember yet another IP address. Long story short, I installed this exactly how I described above in the "Steps for Setup/Install" section.

Problems/Issues Faced
--------------------------------
My current dev environment includes a Vagrant/VirtualBox/Homestead setup so I'm not use to working with Docker. I had a bit of a hill to climb on the first day of this code challenge with getting up to speed with what Docker is and how to use it. The biggest weird issue I ran into was declaring the volume mapping within the `docker run` command. I was initially putting it at the end like so:
`run -it --name prestashop-container-name -p 8080:80 -p 3306:3306 -d prestashop/prestashop:1.6.1.9 -v ~/Desktop/Development/code-challenge/modules:/var/www/html/modules` and Docker wouldn't even start the container but for some weird reason Kitematic would. I went on Stack Overflow and asked the community there for a little help and between the questions I received and me trying out different solutions/debugging I figured out that I needed to move the volume mapping somewhere more towards the beginning of the command. Overall I feel like Docker is a rally useful tool and I'm glad I now have another tool to use in the world of web development.

I didn't really have any issues with creating the Prestashop module even though I'm also new to the Prestashop world as well. From what I've seen Prestashop is pretty straight forward and paradigm is very similar to all of the other extendable platforms that I have worked with before. From start to finish I would say the Prestashop module took me about 4 or 5 hours. I feel that most of that was research due to the fact that I had never seen Prestashop before that and if I had to make another module I could easily cut that down to a couple of hours for a similar speced module. Also of note, the lack of advanced documentation for Prestashop was sort of a bummer.

I have spent a significant amount of time researching best practices for testing Prestashop modules but I haven't found any solid resources as of yet. From what I have researched, I feel that testing could be achieved via RSpec and Capybara. Also, I really don't think that there is too much more time with this Code Challenge to implement any significant testing. Once enough research and implementation of the testing has been done I could see making a Docker image based off of the Prestashop image used in this Code Challenge.
