# Working with CSV data

## Set up

To load CSV file data into p5js, we will start with [this example code](https://p5js.org/reference/#/p5/loadTable) frok p5js website.

```js
// Given the following CSV file called "mammals.csv"
// located in the project's "assets" folder:
//
// id,species,name
// 0,Capra hircus,Goat
// 1,Panthera pardus,Leopard
// 2,Equus zebra,Zebra

var table;

function preload() {
  //my table is comma separated value "csv"
  //and has a header specifying the columns labels
  table = loadTable("assets/mammals.csv", "csv", "header");
  //the file can be remote
  //table = loadTable("http://p5js.org/reference/assets/mammals.csv",
  //                  "csv", "header");
}

function setup() {
  //count the columns
  print(table.getRowCount() + " total rows in table");
  print(table.getColumnCount() + " total columns in table");

  print(table.getColumn("name"));
  //["Goat", "Leopard", "Zebra"]

  //cycle through the table
  for (var r = 0; r < table.getRowCount(); r++)
    for (var c = 0; c < table.getColumnCount(); c++) {
      print(table.getString(r, c));
    }
}
```

As it says on the top comments, create a blank text file in TextEdit(Mac) and paste the data. Before you save, go to `Format > Make Plain Text`. This will remove any formatting and make the text as plain as possible. Now, save with a file extension of `.csv` instead of `.txt`. The CSV file in the example above must be saved in `assets` folder, which should sits right next to your `sketch.js` file.

You can also download the same CSV file [here](../files/mammals.csv).

JavaScript is asynchronous and we cannot predict which data will be loaded first, the sketch may run before it loads the CSV file. That will result in an error. To avoid, p5js has a special function called `preload()`. This will ensure that anything you add into this function will be loaded before the main sketch runs.

```js
function preload() {
  table = loadTable("assets/mammals.csv", "csv", "header");
}
```

## loadTable function
`loadTable()` function is used to load tabular type of data. The data loaded in is now stored in an object called `table`. This object name is not important. You may name it any way you want. Once the object is initialized, we can call many functions on it using the dot notation. Here is [the full reference](https://p5js.org/reference/#/p5.Table). 

The functions used in the example above such as `getRowCount()`, `getColumnCount()`, `getColumn()`, `getString()` are all part of the table object. Here, we are using `print()` to print out to the console to see if it works correctly.

## Display data
If you want to display data on screen, you will use functions like `text()` instead of `print()`. So, let's rewrite `setup()`:

```js
function setup() {
  createCanvas(400, 400);
  background(200);
  
  //cycle through the table
  for (var r = 0; r < table.getRowCount(); r++) {
    var id = table.getString(r, 0);
    var sp = table.getString(r, 1);
    var na = table.getString(r, 2);
    
    push();
    translate(0, 100*r);
    textSize(12);
    text(id + ":", 50, 50);
    text(sp, 65, 50);
    textSize(36);
    text(na, 50, 86);
    pop();
  }	
}
```

## Display one entry at a time
What if, instead of displaying all data at the same time, you want to display one entry at a time? Instead of using for loop, we can access one row at a time by creating a variable `rowIndex`. Here is the updated code:

```js
// https://raw.githubusercontent.com/cdaein/mica-gd355/fall2017/files/mammals.csv
//
// id,species,name
// 0,Capra hircus,Goat
// 1,Panthera pardus,Leopard
// 2,Equus zebra,Zebra

var table;

var rowIndex = 0;

function preload() {
  table = loadTable("https://raw.githubusercontent.com/cdaein/mica-gd355/fall2017/files/mammals.csv", "csv", "header");
}

function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(200);

  var id = table.getString(rowIndex, 0);
  var sp = table.getString(rowIndex, 1);
  var na = table.getString(rowIndex, 2);

  push();
  textSize(12);
  text(id + ":", 50, 50);
  text(sp, 65, 50);
  textSize(36);
  text(na, 50, 86);
  pop();  
}

function mousePressed() {
  rowIndex++;
  if (rowIndex >= table.getRowCount()) {
    rowIndex = 0;
  }
}
```

`mousePressed()` is used to go to the next entry, but this can be automated if necessary. What's important here is that we created the variable `rowIndex` to control which row we want to access.


## Reference
Here is the finixhed code examples:
- http://alpha.editor.p5js.org/cdaein/sketches/H1pz2aIT- (using loop)
- http://alpha.editor.p5js.org/cdaein/sketches/rJg-0TL6- (using index)

[The reference for the table object](https://p5js.org/reference/#/p5.Table) has many other functions and examples, so be sure to check them out and try them yourself. 


