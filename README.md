# someco-platform
Alfresco repo examples from alfresco-developer-series for sdk 3.0.0

-> behvaior test -
1. visit the below link with a document from the repository (change node reference id at last)
http://localhost:8080/alfresco/s/someco/rating-test?nodeRef=workspace://SpacesStore/11f2ca9f-0c92-42ec-a351-8a765e675b09

2. In document details page check for
# of Ratings: 2

-> webscripts: depends on previous tutorial examples (code previously committed)
content-tutorial-repo. This is required because the web script looks for content with the  scr:isActive flag set and that's defined in the SomeCo Content Model that is part of that project.
behavior-tutorial-repo. This is required because it contains a behavior that calculates the average rating for a piece of content and because it has the rating.js server-side JavaScript file that the controllers in this tutorial import.
actions-tutorial-repo. This one is optional. It contains a UI action that is used to set the  scr:isActive flag from within the Alfresco Share user interface.

Starting with SDK 3.0.0, the default is to output a JAR. But we want to output an AMP. To configure the project to generate an AMP, edit the pom.xml and uncomment the maven-assembly-plugin.

Testing:
1.Upload some files to `Company Home/Someco/Whitepapers` (case sensitive)
2.Change the type on the files you uploaded to "SomeCo Whitepaper".
3.If you are going to run the web script as a guest user, make sure that "Guest" is given "Consumer" access to the Whitepapers folder.

The web script descriptor says that this web script requires Guest access or higher. That means you have three options for running the web script:
.Log in to Alfresco (/alfresco) before running the web script.
.Run the web script and then authenticate with a valid user and password when the basic authentication dialog is presented.
.Run the web script with “?guest=true” appended to the URL, like this:
http://localhost:8080/alfresco/service/someco/whitepapers.html?guest=true

result:
http://localhost:8080/alfresco/service/someco/whitepapers.html?guest=true gives

404 Description:	Requested resource is not available.
Message:	No whitepapers found

This in whitepapers.get.js is not getting results
	var whitepapers = search.luceneSearch("PATH:\"/app:company_home/cm:Someco/*\" +TYPE:\"{http://www.someco.com/model/content/1.0}whitepaper\"");

Though I can see two results for below query in node browser lucene search
PATH:"/app:company_home/cm:Someco/*" +TYPE:"{http://www.someco.com/model/content/1.0}whitepaper"

NAME | PARENT | REFERENCE
cm:webscript-test.txt | /app:company_home/cm:Someco/cm:Whitepapers | workspace://SpacesStore/8c369781-ed0a-47ff-b0fc-e2a49335b00b
cm:whitepaper test | /app:company_home/cm:Company_x0020_Home/cm:Someco/cm:Whitepapers | workspace://SpacesStore/dfb28b1a-49c5-41eb-9910-635416f37027

Reason: http://localhost:8080/alfresco/service/someco/whitepapers.html gives result guest is not able to access the documents created by admin

- get ratings script
The link in http://localhost:8080/alfresco/service/someco/whitepapers.html is showing
http://localhost:8080/alfresco/d/d/workspace/SpacesStore/dfb28b1a-49c5-41eb-9910-635416f37027/whitepaper%20test?guest=true

not like
http://localhost:8080/alfresco/s/someco/rating.html?id=11f2ca9f-0c92-42ec-a351-8a765e675b09

-post rating with java based webscript
pass the document id for which rating is enabled
curl -X POST "http://localhost:8080/alfresco/s/someco/rating?id=11f2ca9f-0c92-42ec-a351-8a765e675b09&rating=5&user=jpotts&guest=true"

and retrieve using below from browser
http://localhost:8080/alfresco/s/someco/rating?id=11f2ca9f-0c92-42ec-a351-8a765e675b09

