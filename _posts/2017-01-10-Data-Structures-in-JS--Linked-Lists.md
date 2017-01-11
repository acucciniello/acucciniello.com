---
layout: post
title:  "Data Structures in JS #1: Linked List"
date:   2017-01-10 14:38:20 
---

## Introduction 

Hello! I am starting a series on Data Structures in JavaScript since I have come from a C/C++ background. Today we will start with Linked Lists.

## What is a Linked List?

What even is a Linked List? It is a linear collection of items where each item has access to the next item in the collection.  Here is a picture of what this looks like:

![Linked List Image](/assets/DS-JS/LL/LinkedList.png)

The Linked List has a head property that allows you to access the first item in the list. Each item in the list is referred to as a *node*.  It has access to the actual data of the node, and a property to access the next node in the list.  The last node of the list has a next property equal to null in order to signify the end of the list.  Here is what a Node looks like:

![Node Image](/assets/DS-JS/LL/Node.png) 

## Linked List vs Array?

Right now you are probably asking: Hm, this seems like an array so why would go through the extra work to use and learn about a Linked List? Well there are certain scenarios where one is better than the other:

#### Adding Items

Adding items to array can be easy, as long as the Array has allocated enough space for the items you would like to add and as long as you are adding a new item to the end of the list.  If you are adding an item to the any other location then the end of a list, it will be easier to use a Linked List.  Since you have access to the next item in the list, you can easy move which nodes point to where and allow a new node to come in. In the case of an array, you would need to copy the contents over to a new array and hold them there while you place the new item in its place, and then recopy the items over.  

**Verdict: Use a Linked List, unless adding to the end of an array that has extra space.**

#### Deleting Items

This is similar to adding items.  When deleting an item from an array, you need to copy over the contents to a new array, and then move the items back to cover up the space that was left when you deleted the previous items.  In a Linked List we can just remove the links that a node previous the one being deleted had and make it point to the node after that.  

**Verdict: Use a Linked List for deleting items from a list.**

#### Finding Items in the List

When trying to find items in an array, you can access a specific element by indexing to it using its subscript.  In the case of a Linked List in order to get to a specific element in the list, you need traverse the list up until that point. For larger lists this can take a while.

**Verdict: Use an Array when trying to access specific items in a list**

Ok I hope I have bored you enough with theory, now let's get to coding.

## Implementation
Here we are going to take a look at the code 

### Node

```
// Constructor for Simple Node
var Node = function (value) {
  this.data = value
  this.next = null
}

module.exports = Node

```

As we have explained before a Node is just a simple Data Structure that contains the data that you would like to hold, and access to the next node.

### LinkedList
```
var LinkedList = function () {
  this.head = null
}
```

This is a simple constructor for a Linked List.  It gives you access to head which allows you to access the first element of the list.

### LinkedList.length()

```
LinkedList.prototype.length = function () {
  var length = 0
  var current = this.head
  if (current === null) {
    return length
  } else {
    length = 1
    while (current.next !== null) {
      current = current.next
      length++
    }
    return length
  }
}
```

This is a simple function that calculates the amount of items in a Linked List by traversing the list from head all the way till *node.next = null*.  When a node's next property is null that means that it does not point to another node and you have reached the end of the list.

### LinkedList.push()

```
LinkedList.prototype.push = function (value) {
  var addNode = new Node(value)
  if (this.head === null) {
    this.head = addNode
  } else {
    var current = this.head
    while (current.next !== null) {
      current = current.next
    }
    current.next = addNode
  }
  return
}
```
This is a simple function to add a node to the end of a Linked List otherwise known as *push*.  Here our parameter *value* represents the data that will be added to the end of the list.  First we take the data and make a node from it.  Then if the list is empty we simply have *head* point to the new node created.  If there are items in the Linked List, then we create a new variable called current which will track which node we are on while traversing the list.  Then we loop through until we hit *null* once we hit null, we make the last node in the list point to the new node that was created, effectively adding it to the end of the list.  Here is a visual representation of what is happening during a push.
 
![Push Image](/assets/DS-JS/LL/Push.png)

### LinkedList.insert()

```
LinkedList.prototype.insert = function (index, node) {
  var previous = null
  var current = this.head
  if (this.length() < index || index < 0) {
    return 'The index does not exist in the Linked List'
  } else if (this.head === null) {
    return 'The list is empty use push instead'
  } else {
    for (var i = 0; i <= index; i++) {
      previous = current
      current = current.next
    }
    previous.next = node
    node.next = current
    return 'Inserted a node at index ' + index + ' with value of ' + node.data
  }
}
```

This is similar to *push*, except here the goal is to add the Node at the specific *index* given as a parameter.  We create a *previous* variable that points to the node before the current node.  Then we create a *current*	 variable to keep track of where we are in the list.  Then we traverse the list until we hit the given index.  Once there we allow the previous node to point to the new node by doing *previous.next = node* and then have the new node point to the current node by doing *node.next = current*. Now the node was added to the specific index given.


### LinkedList.remove()

```
LinkedList.prototype.remove = function (index) {
  var previous = null
  var current = this.head
  console.log(this.length())
  if (this.length() < index || index < 0) {
    return 'The index does not exist in the Linked List'
  } else if (this.head === null) {
    return 'You cannot remove items from an empty list'
  } else {
    for (var i = 0; i <= index; i++) {
      previous = current
      current = current.next
    }
    if (current === null) {
      previous.next = null
    } else {
      previous.next = current.next
      current = null
    }
    return 'We removed the node of index ' + index
  }
}
```
This is similar to *insert*. We are given and index of the node we would like to remove, so we take that index, traverse through the Linked List using previous and current variables to get there.  Once we get to the specific index we move previous's property of next to point to the node that will be deleted's next property by doing: *previous.next = current.next*.  Then we make the current node equal to null to remove any pointers it had to the Linked List.  Here is a visual representation of what happens when you remove an item from a list:

![Remove Image](/assets/DS-JS/LL/Remove.png)


## Conclusion

There you have it, a quick guide on Linked Lists in JavaScript.  I hope this helps anyone who was struggling to understand this data structure.  If you have any questions, feel free to reach out to me by email: antonio.cucciniello16@gmail.com or on twitter @antocucciniello.  The full code for this post can be found on my GitHub [here][DSJSCode].  

[DSJSCode]: https://github.com/acucciniello/data-structures/tree/master/js/linked-list


 

