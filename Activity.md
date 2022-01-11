SD05 JavaScript & Front-End Web Development | Asynchronous JavaScript
Activity: Building a Confirmation Modal
In this activity, we will build a document manager for Northwest Marketing Services, a company based in Seattle. We have a list of documents and want to add a delete button for users to delete files. The problem is that some of the documents are very important, so we need to ensure the user intends to delete a document before completing their request. To accomplish this, we will use a confirmation modal, a pop-up that will ask users if they are sure before deleting a document. While the confirmation modal is displayed, we need our code to wait until the user clicks confirm or cancel, so we can respond to their selection. We will use promises to ensure our logic for deleting a document will wait while other parts of our code, like the confirmation modal, continue to run.

1. Add an Event Listener to the Delete Button
The code below iterates through an array of document names using .forEach and adds an li element to the page for each document. We also add a delete button to each li. Our next step is to add an event listener to the delete button, so we can respond to a user who is trying to delete a document. Since we do not yet know what we want the event listener to do, we will leave it empty for now.

```
let documents = [ "creative writing.docx", "taxes.pdf", "syllabus.pdf", "final project.pptx" ]
let documentUl = document.querySelector('.document-list')

documents.forEach(function (documentName) {
  let documentLi = document.createElement("li");
  documentLi.textContent = documentName;

  let deleteButton = document.createElement("button");
  deleteButton.textContent = "Delete";
  documentLi.append(deleteButton);

  // Add event listener here.
  deleteButton.addEventListener("click", () => {});

  documentUl.append(documentLi);
});
```

2. Create a Confirmation Modal
Now that we can detect when a user is trying to delete a document, we want to display a confirmation modal. To practice separation of concerns, we will create our confirmation modal in a new function and then invoke that function in our event listener. Below, we already have a function that creates a div to represent the modal and styles it. Our next step is to add two buttons to the modal: a confirm button and a delete button. We will add event listeners to these buttons as well.

```
function getUserConfirmation(){
    let modalDiv = document.createElement('div')

    modalDiv.style.padding = '10px';
    modalDiv.style.position = 'absolute';
    modalDiv.style.backgroundColor = 'white';
    modalDiv.style.border = '1px solid black';
    modalDiv.style.width = '50%';
    modalDiv.style.height = '100px';
    modalDiv.style.left= '20%';
    modalDiv.style.top = '20px';


    let modalHeaderDiv = document.createElement('div')
    modalHeaderDiv.textContent = `Are you sure you want to delete this document?`

    modalDiv.append(modalHeaderDiv)

    // Add a 'confirm' button and a 'cancel' button here.
    let confirmButton = document.createElement('button');
  	confirmButton.textContent = "Confirm";
  	confirmButton.addEventListener('click', () => {
    	
    }) 
  	modalDiv.append(confirmButton)

  	let cancelButton = document.createElement('button');
	cancelButton.textContent = "Cancel";
  	cancelButton.addEventListener('click', () => {
    	
    }) 
  	modalDiv.append(cancelButton)

    document.body.append(modalDiv)
}

// We will invoke our function just to test it. (We will get rid of this later.):
getUserConfirmation()
```

3. Invoke getUserConfirmation
Now that we have a function to display the confirmation modal, we will invoke it when the user clicks a delete button.

```
let documents = [ "creative writing.docx", "taxes.pdf", "syllabus.pdf", "final project.pptx" ]
let documentUl = document.querySelector('.document-list')

documents.forEach(function(documentName){
    let documentLi = document.createElement('li');
    documentLi.textContent = documentName;

    let deleteButton = document.createElement('button')
    deleteButton.textContent = 'Delete'
    documentLi.append(deleteButton)

    deleteButton.addEventListener('click', function(){
        // Invoke 'getUserConfirmation' here.
      getUserConfirmation();
    })

    documentUl.append(documentLi)
})

function getUserConfirmation(){
    let modalDiv = document.createElement('div')

    modalDiv.style.padding = '10px';
    modalDiv.style.position = 'absolute';
    modalDiv.style.backgroundColor = 'white';
    modalDiv.style.border = '1px solid black';
    modalDiv.style.width = '50%';
    modalDiv.style.height = '100px';
    modalDiv.style.left= '20%';
    modalDiv.style.top = '20px';


    let modalHeaderDiv = document.createElement('div')
    modalHeaderDiv.textContent = `Are you sure you want to delete this document?`

    modalDiv.append(modalHeaderDiv)

    let confirmButton = document.createElement('button')
    confirmButton.textContent = 'Confirm'

    let cancelButton = document.createElement('button')
    cancelButton.textContent = 'Cancel'

    modalDiv.append(confirmButton, cancelButton)

    confirmButton.addEventListener('click', function(){
    })

    cancelButton.addEventListener('click', function(){
    
    })

    document.body.append(modalDiv)
}
```

4. Make getUserConfirmation Asynchronous
Now comes the tricky part: After invoking getUserConfirmation, we want to get the user's response. If they clicked confirm, delete the document. The problem is that getUserConfirmation will not know the user's choice until later, when one of the event listeners set up earlier is invoked. To solve this problem, we will have getUserConfirmation return a promise. This promise will report later whether the user wants to actually delete the document.

```
let documents = [ "creative writing.docx", "taxes.pdf", "syllabus.pdf", "final project.pptx" ]
let documentUl = document.querySelector('.document-list')

documents.forEach(function(documentName){
    let documentLi = document.createElement('li');
    documentLi.textContent = documentName;

    let deleteButton = document.createElement('button')
    deleteButton.textContent = 'Delete'
    documentLi.append(deleteButton)

    deleteButton.addEventListener('click', function(){
        getUserConfirmation()
    })

    documentUl.append(documentLi)
})

function getUserConfirmation(){
    let modalDiv = document.createElement('div')

    modalDiv.style.padding = '10px';
    modalDiv.style.position = 'absolute';
    modalDiv.style.backgroundColor = 'white';
    modalDiv.style.border = '1px solid black';
    modalDiv.style.width = '50%';
    modalDiv.style.height = '100px';
    modalDiv.style.left= '20%';
    modalDiv.style.top = '20px';


    let modalHeaderDiv = document.createElement('div')
    modalHeaderDiv.textContent = 'Are you sure you want to delete this document?'

    modalDiv.append(modalHeaderDiv)

    let confirmButton = document.createElement('button')
    confirmButton.textContent = 'Confirm'

    let cancelButton = document.createElement('button')
    cancelButton.textContent = 'Cancel'

    modalDiv.append(confirmButton, cancelButton)


    document.body.append(modalDiv)

    // Return a new Promise here.
  	return new Promise((resolve, reject) => {
  		confirmButton.addEventListener('click', function(){
			resolve(true);
          	modalDiv.remove()
      	})

    	cancelButton.addEventListener('click', function(){
          	resolve(false);
         	modalDiv.remove();
      	})
    })
}
```

5. Use .then to Wait for getUserConfirmation
Now that getUserConfirmation returns a promise, we can use .then inside our delete button's event listener to wait for the user's response and delete the document, if necessary.

```
let documents = [ "creative writing.docx", "taxes.pdf", "syllabus.pdf", "final project.pptx" ]
let documentUl = document.querySelector('.document-list')

documents.forEach(function(documentName){
    let documentLi = document.createElement('li');
    documentLi.textContent = documentName;

    let deleteButton = document.createElement('button')
    deleteButton.textContent = 'Delete'
    documentLi.append(deleteButton)

    deleteButton.addEventListener('click', function(){
        getUserConfirmation().then(confirmed => {
        	if (confirmed) {
            	documentLi.remove();
            }
        })
    })

    documentUl.append(documentLi)
})

function getUserConfirmation(){
    let modalDiv = document.createElement('div')

    modalDiv.style.padding = '10px';
    modalDiv.style.position = 'absolute';
    modalDiv.style.backgroundColor = 'white';
    modalDiv.style.border = '1px solid black';
    modalDiv.style.width = '50%';
    modalDiv.style.height = '100px';
    modalDiv.style.left= '20%';
    modalDiv.style.top = '20px';


    let modalHeaderDiv = document.createElement('div')
    modalHeaderDiv.textContent = 'Are you sure you want to delete this document?'

    modalDiv.append(modalHeaderDiv)



    let confirmButton = document.createElement('button')
    confirmButton.textContent = 'Confirm'

    let cancelButton = document.createElement('button')
    cancelButton.textContent = 'Cancel'

    modalDiv.append(confirmButton, cancelButton)

    document.body.append(modalDiv)

    return new Promise(function(resolve){
        confirmButton.addEventListener('click', function(){
            resolve(true)
            modalDiv.remove()
        })

        cancelButton.addEventListener('click', function(){
            resolve(false)
            modalDiv.remove()
        })
    })
}
```
