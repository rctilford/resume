---
title: Sorting an HTML Table
author: Rhys Tilford
description: Read about how I used javascript to populate and sort an HTML table.
date: 11/5/2023
image: Table.png
categories: [HTML, JS, Programming]
---

### The Data Input

For this challenge, we recieved an array of objects that contained metadata about artists who perform a music genre known as bhangra. Each object contained the three attributes:

 - *name* referring to the name of each artist.
 - *birthYear* referring to the year each artist was born.
 - *link* referring to a youtube link for a video of that artist's work.

##### This is the array we were given.

```
const artists = [
  {
    name: "Ms Scandalous",
    birthYear: 1985,
    link: "https://www.youtube.com/watch?v=2FPivlfvxu0"
  },
  {
    name: "Juggy D",
    birthYear: 1981,
    link: "https://www.youtube.com/watch?v=1jAc_-FVjdI"
  },
  {
    name: "Sukhbir Singh",
    birthYear: 1969,
    link: "https://www.youtube.com/watch?v=HiprNF9Jad0"
  },
  {
    name: "Abrar-ul-Haq",
    birthYear: 1989,
    link: "https://www.youtube.com/watch?v=-lnnVIP7FEc"
  },
  {
    name: "Rishi Rich",
    birthYear: 1970,
    link: "https://www.youtube.com/watch?v=O95-w2gACuA"
  }
]
```

### Step 1. Making the Table

First, the students must populate a given table element to hold the data. We chose to accomplish this by systematically accumulating HTML tags as string surrounding metadata from the array and inserting the accumulation into the table element.

```
function populateTable(array) {

  let table = document.getElementById('bhangra'); // this code selects the table

  // declare a string to hold the inner HTML code for the table
  let contents = "<tbody>";

  // add the header row
  contents += `<tr><th>name</th>`;
  contents += `<th>birthyear</th>`;
  contents += `<th>link</th></tr>`;


  array.forEach(function (artist) {
    contents += `<tr>`; // open the row
    contents += `<td>${artist.name}</td>`;
    contents += `<td>${artist.birthYear}</td>`;
    contents += `<td><a href= "${artist.link}" target="_blank">${artist.link}</a></td>`;
    contents += `</tr>`; // close the row
  })


  // close out the body of the table
  contents += "</tbody>";

  table.innerHTML = contents; // this code populates the table with the contents string

};

// call the function to populate the table
populateTable(artists);
```

### Step 2. Initializing Button Elements

Second, we created a general function for creating button elements.

```
function createButton(id, label, parent) {
 
  // create a new button element
  const newbutton = document.createElement("button");
 
  // add the text to the button
  newbutton.innerText = label;
 
  // add the id to the button
  newbutton.id = id;
 
  // append the button to the parent
  parent.appendChild(newbutton);

  // return the button
  return newbutton;

};
```
### Step 3. Initialize All Buttons

Third we used the `createButton()` function to make buttons for each of the sorting actions we want to enable.

```
const nameButton = createButton("name-button", "Sort by Name", document.getElementById("sorting"));

const yearButton = createButton("year-button", "Sort by Year", document.getElementById("sorting"));

const randomButton = createButton("random-button", "Shuffle", document.getElementById("sorting"));
```

### Step 4. Create Event Listeners
Fourth, we needed to create event listeners for each button so the javascript code to sort the data would run when the buttons are pressed.

###### Info Block
The functions referrenced in each event listener are referenced later in the document. This is not problematic because JavaScript uses asynchronous programming.


```
nameButton.addEventListener('click', function () {
  artists.sort(byName);
});

yearButton.addEventListener('click', function () {
  artists.sort(byYear);
});

randomButton.addEventListener('click', function () {
  artists.sort(shuffle);
});
```
### Step 5. Functions for Sorting

Fifth and finally, we needed to create a function for each sort method.

```
// first a function to sort by name:
function byName(a, b) {

  populateTable(artists.sort(function (a, b) {
    if (a.name < b.name) return -1;
    if (a.name > b.name) return 1;
    return 0;
  }));

};

// second a function to sort by year:
function byYear(a, b) {

  populateTable(artists.sort(function (a, b) {
    if (a.birthYear < b.birthYear) return -1;
    if (a.birthYear > b.birthYear) return 1;
    return 0;
  }));

};

// third a function to shuffle the rows:
function shuffle() {

    for (let i = artists.length - 1; i > 0; i--) {
      let j = Math.floor(Math.random() * (i + 1));
      [artists[i], artists[j]] = [artists[j], artists[i]];
    }

    populateTable(artists);

};
```

*Some parts of the above code were adapted from suggestions by Github Copilot.*

### If you would like to see the code in action click [here](https://rctilford.github.io/CSC324/).