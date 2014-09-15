Documentation
-	index.php: this is the start of the experiment, asks for the id of the participant and sends it to page1.php
-	page1.php: this is the pre-experiment questionnaire for getting demographic data. Change the form elements to add new data to be retrieved later… sends data as post to page2.php
-	page2.php:
  o	Lines 12 – 18: This is where the data from page1.php is put in cookie form with key: demodata_[ name of variable ] and value... expiration time is 3 hours. You can change this part so that you can save the data to MySQL database.
  o	Lines 33 – 40: This is where you set the number of blocks (experiment sessions) for a user.
    	The example given is set to 2. That means if you choose (in this page) to start with ACP, then the experiment will be done 2 times:
      •	Block 1:
        o	ACP
        o	XWindow
      •	Block 2:
        o	ACP
        o	XWindow
    	Change the variable $max_blocks to any integer from 1 to infinity to determine the number of blocks in your experiment.
        o	After the experimenter has picked the starting system, the data is taken to page3.php
    	You can also modify this to just automatically “randomize” the system based on user id or anything. Just make sure you send either “acp” or “xwindow” as value for the variable “interface”
-	page3.php: this is a pause page before the actual experiment. You can put instructions here so that the user can understand what to do for your experiment. When they click start, they will be taken to interface.php with the parameters to customize the experiment
-	interface.php: this will call testenv.html as the interface of the experiment
  o	testenv.html: this is the interface of the experiment, it will call several javascript files to startup the system
    	js/jquery-2.0.2.min.js: this is the core library jquery to do jquery stuff
    	js/jquery-ui-1.10.3.custom.min.js: this is the core library jquery ui to do ui manipulation (mostly dragging)
    	js/caretposition.js: this checks the caret position of the text editor (DO NOT CHANGE)
    	js/base_class.js: this is the basic class used for inheritance purposes (DO NOT CHANGE)
    	js/index.mins.js: this is the main javascript that takes in interaction and manipulates everything
      •	Lines 17 – 43: jQuery.fn.highlight – highlights the text
      •	Lines 45 – 54: jQuery.fn.removeHighlight – removes highlights from text
      •	Lines 65 – 72: replaceURLs
        o	Parameter: string
        o	It just replace the URLs and makes it into an anchor tag
      •	Lines 75 – 78: capFirst
        o	Parameter: string
        o	Capitalizes the first letter of the string
      •	Lines 81 – 87: makeId
        o	Parameter: length
        o	This is used to create ids for the objects in the UI for easy differentiation and access in the object mapping in javascript. It takes in the length of the string id to be generated and spews out the id
      •	Lines 91 – 105: toStringNum
        o	Parameter: num
        o	Sanitize input as a number input
      •	Lines 107 – 126: toFloat
        o	Parameter: string
        o	Sanitize string (like 2/3) as a float form
      •	Lines 130 – 138: saveElement
        o	Parameter: parent element, element type, id of element, classes included, and other attributes in javascript object notation
        o	Creates an element and returns a jquery object
      •	Lines 141 – 150: saveElementAfter
        o	Parameter: the last child element, element type, id of element, classes included, and other attributes in javascript object notation
        o	Creates an element after the last child element and returns a jquery object
      •	Lines 153 – 161: withinArea
        o	Parameters: x of mouse, y of mouse, x2 of topleft of bounding box, y2 of bounding box, length of sides
        o	Returns if mouse is in bounding box
      •	Lines 164 – 166: br
        o	Creates element <br/>
      •	Lines 169 – 193: createElement
        o	Called by saveElement and saveElementAfter to create an element
      •	Lines 203 – 215: $.cachedScript
        o	Caches dynamically loaded script
      •	Lines 217 – 635: dataModel class
        o	Lines 218 – 228: init
          	Initializes the class
        o	Lines 230 – 236: run
          	Runs the class
          	Lines 237 – 241: loads a text file from data folder that is a list of texts to be loaded as test data for the experiment… the default is data/data.txt
          	Lines 256 – 260: loads a text file from extrajs folder that is a list of javascript files to be loaded for taking in user data… the list default is in extrajs/extrajs.txt
          	Lines 262 – 267: loads a json file from the task folder that is a list of tasks in json format… the default is task/tasks.json
          	Line 278: loads all data files from data.txt using loadFile function in the dataModel class and puts it in an array of articles
          	Lines 283 – 319: processes the articles using the processTextFile function in the dataModel class
          	Lines 321: puts the tasks.json in tasks object
          	Lines 368: puts the extra javascript files from extrajs.txt in extrajs variable
        o	Lines 378-396: processJScript
          	Process the dynamically loaded javascript so that it can be used in the system
        o	Lines 398 – 480: processTextFile
          	Parses the data files into a word barrel type (seen in the pagerank algorithm of Google). This is the most basic type of implementation: 
            •	Word (as key)
              o	Array of Article indices where the word is found
              o	Array of Paragraph indices where the word is found
              o	Array of position of paragraph
              o	Array of Sentence indices where the word is found
              o	Array of position of sentence
              o	Array of position of word in a sentence
        o	Lines 482 – 539:  wordIndex – adds or deletes word in database
        o	Lines 540 – 542: short word of wordIndex
        o	Lines 544 – 605: searchWord, given a partial word or phrase, search the sentence or part of sentence with the word or phrase at the start of it
      •	Lines 647 – 692: buttonClass – creates a button to interact with a given callback function when clicked
      •	Lines 694 - 712: ButtonIcon – creates a button with an icon or title
      •	Lines 714 – 808: WindowdElement – creates a standard window class
      •	Lines 809 – 835: windowArticles – creates a window from windowElement for the data articles
      •	Lines 836 – 1531: AutoComPaste – creates a window form text area that has the AutoComPaste interaction
        o	Lines 837 – 845: init the autocompaste model
        o	Lines 847 – 867: runs the window, binds events focus, click, blur, keydown and keypress in textarea to functions
      •	Lines 869 – 881: on_click – gets caret position in the text area when user clicked on the text area
      •	Lines 889 – 910: getCaret – uses the caret function of javascript at the top to get the caret position
      •	Lines 912 – 941: keypressed – this is called when user clicks on any thing in the keyboard
        o	If text area is focused
          	If character is a new line, then query word is reset
          	If query word is “”, then caret start is in 0 position
          	When typing, characters are added in the query word
          	Line 935: search current query word
      •	Lines 942 – 1050: keydowned – this is called when user presses down on a keyboard
        o	If text area is focused
          	If backspace, set caret position back and delete last letter in query word, then search query word again
          	If left key and pasted (from dropdown), remove sentence
          	If right key and pasted (from dropdown), add sentence
          	If down key and pasted (from dropdown), add paragraph, if dropdown is shown then show next sentence in dropdown list in text area
          	If up key and pasted (from dropdown), remove paragraph. If dropdown is shown then show previous sentence in dropdown list in text area
          	If escape and pasted (from dropdown), revert back to original text
          	If “]”, add word
          	If space bar or enter and pasted (from dropdown), sets the sentence or paragraph as new text and resets query word. Also triggers the event “acp_paste” for calling logging
          	Lines 1543 – 1606: viewerUI class – UI setup
          	Lines 1607 – 1619: ResultObject
          	Lines 1621 – 1625: log function – puts it in the logdata array everything that is being logged. Call this to put any data that needs to be logged
          	Lines 1635 – 1696: checkNextTask – this is called when the user clicks on the button “Next Task”
      •	Line 1656: Sends data to setCookie.php, you can change the file to send if you want to send it to a php file that gets the data and saves it in a MySQL database
      •	Line 1658 – 1663: When setCookie.php is done, it will just print the logged in data back in your console… you can change this as well
-	Page4.php – this is called after you have done the interface.php, it then checks if all blocks are done. If it is not done, it autosends you to page3.php. If it is done, autosends you to page5.php
-	Page5.php – this is called to show you that you can download the data using the generate.php
-	Generate.php – you can modify this to get the data from your MySQL database and prints it as a file

Extra files
-	extrajs/default.js – modify this to bind events to functions in your defaultlib.js
-	extrajs/defaultlib.js – this is an example library of functions being called when an event (like “acp_paste”) is being called. It has to be bound in the default.js so that the function can be called. Modify this so that you can log in more data needed
