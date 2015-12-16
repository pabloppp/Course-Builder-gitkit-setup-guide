#GITKIT - Google CourseBuilder 1.9
*guide by Pablo Pernías* 


##Step by step setup guide
###1 - The initial stuff (The easy part)
After cloning the google coursebuilder from this repository and creating your first course (I will asume that you already know how to do that), let's make it viewable without registration (it's not necesary but will make things a little easier).

In order to do that, go to your CourseBuilder's dashboard, then click in `Create > Outline` in the left menu, and click the **lock** icon next to your course. it will then show an unlocked icon:

![](https://dl.dropboxusercontent.com/u/34394156/images/coursebuilder/Captura%20de%20pantalla%202015-11-14%20a%20las%202.03.03.png)

The next thing you must do is open the `app.yaml` file in the root folder of your project, and scroll until you see the lines:

`modules.gitkit.gitkit=disabled`

`modules.oauth2.oauth2=disabled`

And edit them to leave them like this:

`modules.gitkit.gitkit`

`modules.oauth2.oauth2`

Note that we just removed the 'disabled' part, that means that gitkit and OAuth2 modules will be enabled.

*--Don't deploy the project yet--*
###2 - Am I the ADMIN? (The ??? part)
This second step is quite easy: We will make sure that we are the page Admin. This is important because if we're not... Well... We just cannot do anything.

Open the file `config.yaml` that you can find in `your_project_dir/modules/gitkit/resources/`

You will see a list of emails under the title **admins**. You have to add your google email (the one associated with your developer account) to that list.
You can erase the example email if you want to.

It should look like this:

![](https://dl.dropboxusercontent.com/u/34394156/images/coursebuilder/Captura%20de%20pantalla%202015-11-14%20a%20las%202.17.37.png)

!! Do **NOT** set the 'enabled' field to true! Not yet!

NOW you should **deploy** your app (if you are working locally you probably just need to RUN the project from the App Engine Launcher).

Once again go to your CourseBuilder's dashboard. Then click in `Site admin > Site settings` in the left menu.

You will see a big grid with a lot of variables, it's values and a description.

For now just let's focus on the first one: `Admin email addresses`.
Click on **Override** and then, in the input form, write your email again (yes, the same one that you wrote before).

!! AND DONT FORGET TO SET THE STATUS TO **Active** OR ELSE IT WILL NOT MAKE ANY EFFECT

!Note that here you can add **as many emails** as you want. That will grant those users with **Admin privileges**, and they should be able to access the dashboard, even though I have not tested that.

Finally click `Save` then `Close`.

###2 - The Google API stuff (The long part)
Now comes the "hardest" part of this guide. We must configure out API and Credentials and tell them to GitKit.

In the same page that you were before (that is `Site admin > Site settings`) scroll down until you see a bunch of variables beggining with `GITKit module ...`

There's an explanation of what you must put there in the left part of the grid, but I'll gide you so you don't even have to squeeze your brain a little (yay!)

Go to your google developper's dashboard [or just click here](https://console.developers.google.com).

Take a look at the the upper bar to make sure that you're in the right project, In this case, my project is called `coursebuilderGitkitI18N`:

![](https://dl.dropboxusercontent.com/u/34394156/images/coursebuilder/Captura%20de%20pantalla%202015-11-14%20a%20las%202.35.19.png)

We are at the right place!

Open the left side menu and go to `API Manager`.

~~~ *Field 1/4* ~~~

Under *Overview* you'll see a search bar. Just type `Identity Toolkit API` and it will appear.
Click on it and then click on `Enable API`.
For now just leave it that way.

Now, on the left side menu select `Credentials`.
Then click the blue button that says `Add credential` and select `API Key` from the dropdown list.

In the popup, select `Browser key`.
A form will show up. Here you can leave the default *Name*.

In the *Accept requests from these HTTP referrers* field you must write your app's domain followed by an asterisk.

Example: `https://my_coursebuilder_site.appspot.com/*`

If you are running your tests locally you probably also want to add `http://localhost:8080/*` to that list.

*If you find any problem, you can also leave this field blank or write `https://*.appspot.com/*`. This makes the website less secure.*

Then click on `Create` and a token will be displayed. **COPY** that token and go to your cousebuilder dashbard again, to the `Site admin > Site settings` page, and paste it in the `GITKit module browser API key` field (again, remember to set the status to **Active** before saving).

~~~ *Field 2/4* ~~~

Go back to the developpers page and click on `add credentials` again. 
This time though you must select the option `API key > Server key`.

Here you can leave all the field with the default values and click `Create`right away.

A new token will apear. *COPY* it, go to your coursebuilder `Site settings` and paste it in the `GITKit module server API key` field.  

~~~ *Field 3/4* ~~~

Back in the developpers page and click on `add credentials` for a third time. But now select `OAuth 2.0 client ID` from the dropdown.

It might show you a warning telling you to set a product name: 
![](https://dl.dropboxusercontent.com/u/34394156/images/coursebuilder/Captura%20de%20pantalla%202015-11-14%20a%20las%203.00.09.png)
Just follow the steps and you will be back there when the name is set. 

Example using my course name:
![](https://dl.dropboxusercontent.com/u/34394156/images/coursebuilder/Captura%20de%20pantalla%202015-11-14%20a%20las%203.01.36.png)

Now select `Web application`from the list.
A form will appear, you can leave the default `Name`.

In the field `Authorized JavaScript origins` you just have to write your site's main url:
`https://my_coursebuilder_site.appspot.com` (and/or localhost if you are running in local).

In the field `Authorized redirect URIs` you have to write your url followed by the string `/modules/gitkit/widget`

Example:
`https://my_coursebuilder_site.appspot.com/modules/gitkit/widget`

Click on `Create` and *COPY* the *Client ID* (just where it says *Here is your client ID*).

Finally, click on `Download JSON` and save the file cliente_secret_xxxxxxx-xxxxxx.json, we will use it later.

Go back to your coursebuilder `Site settings` and paste it in the field `GITKit module client ID`.

~~~ *Field 4/4* ~~~

For the final field, go to your developpers page and click on `add credentials` (yes... AGAIN!!! @_@' ).

Now select the option `Service account` from the dropdown. Then select `JSON` as the *Key type* and click on `Create`.
This will download a .json file to your computer, you can save it somewhere safe, but what you need to do is *OPEN* it with any text editor and *COPY* its content.

Then, go back to your coursebuilder `Site settings` and paste all that in the field `GITKit module service account JSON`

!!! REMEMBER AGAIN that whenever you save a variable you have to set it's status to **Active** or it will not make any difference.

OK! So now you have all the variables that you needed, hurray!!!

**BUT WAIT! There are still a few things you must do!**

~~~ *Put it all together* ~~~

Go back to your developpers page one LAST time.

In the left side menu select `Overview`, then under the title select the tab named `Enabled APIs` and click in the small black *COG* icon that appears nex to `Identity Toolkit API`.

We must now complete that form.
In the first field `Widget URL` you must select the only option that shows up in the dropdown (that's an easy one).

In the `Sign-in success URL` field you can write your url followed by the string `/modules/gitkit/widget`

Example:
`https://my_coursebuilder_site.appspot.com/modules/gitkit/widget`

And in the `Sign-out URL` you can write the same thing.

Then, in the *Providers* list, check the services that you want your users to be able to log in.
In the *Google* provider you must set the 'Rollout'. You can just set it to 100%.

*Note that if you select any provider other that Google you'll have to provide the credentials for that service. We will not cover that in this guide but it's quite easy, believe me!*

Click on the `Save` button.

Finally, you have to edit the file client_secrets.json from OAuth2 module (coursebuilder_directory/modules/oauth2/client_secrets.json) and replace it's content with the file you downloaded before when created the `OAuth 2.0 client ID` credential.


###3 - Almost there!!! (The satisfaction)

We're almost finished, I promise!!!
The only thing we need to do now is activate the **gitkit** login module.

For this, open the file `config.yaml` that you can find in `your_project_dir/modules/gitkit/resources/`

**NOW** you can set the the `enabled` field to `True`!!!

![](https://dl.dropboxusercontent.com/u/34394156/images/coursebuilder/Captura%20de%20pantalla%202015-11-14%20a%20las%203.29.15.png)

And now **DEPLOY** your project (or run it locally).

Go to your site and test it by **Login**, you will see a page similar to this one:

![](https://dl.dropboxusercontent.com/u/34394156/images/coursebuilder/Captura%20de%20pantalla%202015-11-14%20a%20las%204.32.26.png)

I guess frome here you're good to go on your own. 

**LUCK**!!!