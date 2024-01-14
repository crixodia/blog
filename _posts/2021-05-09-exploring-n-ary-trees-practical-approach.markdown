---
title: "N-ary Trees: A Practical Approach"
categories: [Data Structures, Java]
tags: [N-ary Trees, Java, GUI Development, Parsing]
date: 2021-05-09 15:14:00 -0500
math: false
mermaid: true
pin: false
img_path: /assets/img/posts/n-ary-trees-practical-approach/
image: n-ary-trees-post.webp
---
Unlike binary trees, which have only two child nodes, n-ary trees can have any number of child nodes. In this post, we will dive into the world of n-ary trees and explore their application in managing family trees.

## Understanding N-ary Trees

In essence, an n-ary tree is a non-linear abstract data structure defined recursively with a set of nodes. These nodes maintain a list that points to other nodes.

```mermaid
---
title: "N-ary tree"
---
graph TD
    A[Root] --> B(Child 1)
    A --> C(Child 2)
    A --> D(Child 3)
    A --> E(...)
    A --> F(Child N)
    B --> G(Child 1)
    B --> H(Child 2)
    B --> I(Child 3)
    B --> J(...)
    B --> K(Child N)
```

## Family Tress and N-ary Trees

One compelling application of n-ary trees is managing family trees. N-ary trees perfectly fit the requirements of family trees, offering a hierarchical structure with parent-child relationships. Below, you can see an example of a family tree inspired by Greek mythology.

![Family tree sample](greek-family-tree-light.webp){: .light }
![Family tree sample](greek-family-tree-dark.webp){: .dark }
_Greek Family Tree_

## Defining the Objects

To create a family tree, we will implement a generic object. Additionally, we'll define the tree structure as outlined below. It's important to note that the methods implemented in the tree class can vary depending on your specific needs and the type of objects used as values in the nodes.

### Node Implementation

The implementation of our tree node is straightforward. It consists of a parent object, a root object, and a list of child objects. To add nodes, you can use the following method in Java:

```java
public Node add(Object o){
    Node newNode = new Node(o);
    this.children.add(newNode);
    return newNode;
}
```

>You can review the full [Node class implementation here.](https://github.com/crixodia/nary-family-tree/blob/master/ArbolGen/src/CapaNegocio/Node.java)
{: .prompt-info }

### Tree Implementation

The main challenge in this project is implementing the tree structure. Since trees can be stored in files, you'll need to design a parser to translate the tree into text and vice versa. Additionally, to display the tree in a graphical user interface (GUI), you'll require a parser that can convert it into a `DefaultMutableTreeNode` type object.

For this project, I relied on the following pseudocode to understand the process (assuming a basic knowledge of stacks):

```text
define stack
stack.push(first line)
while objects exist
    S1 = stack.peek()
    S2 = read object
    if depth(S1) < depth(S2)
        S1.addChild(S2)
        stack.push(S2)
    else
        while depth(S1) >= depth(S2) and stack.size() >= 2
            stack.pop()
            S1 = stack.peek()
    S1.addChild(S2)
    stack.push(S2)
return stack.get(0)
```

### File Handling: Saving and Opening Trees

Reading and saving trees in files can be simplified by including the parent of each item. Each node is represented on a separate line in the following format: `parent:value`. Additional attributes of the object are specified with colons.

For example:

```text
Aang:Katara:0:0
Aang:Katara,Tenzin:Pema:0:0
Tenzin:Pema,Meelo::0:0
Tenzin:Pema,Jinora::0:0
Tenzin:Pema,Ikki::0:0
Aang:Katara,Bumi::0:0
Aang:Katara,Kya::0:0
```

To read these lines, you can extract the parent and node values and then use the `addNewNode(parent, value)` method, such as `addNewNode(Aang, Tenzin)`.

To save trees, traverse the entire tree in a pre-order fashion and concatenate the parent and node values in the specified format.

```mermaid
---
title: Pre-order Traversal
---
graph TD
    subgraph Tree
        A((A)) --> B((B))
        A --> C((C))
        B --> D((D))
        B --> E((E))
        C --> F((F))
        C --> G((G))
        D --> H((H))
        D --> I((I))
        E --> J((J))
        E --> K((K))
        F --> L((L))
        F --> M((M))
        G --> N((N))
        G --> O((O))
    end
    Tree --> P
    subgraph P[Traversal]
        x([ A - B - D - H - I - E - J - K - C - F - L - M - G - N - O ])
    end
```

>You can find methods for searching, removing, calculating depth, and modifying nodes in the [Tree class](https://github.com/crixodia/nary-family-tree/blob/master/ArbolGen/src/CapaNegocio/Tree.java).
{: .prompt-info }

### Ensuring Object Uniqueness

To handle cases where a child has the same ID as its father, we ensure uniqueness through the father's spouse. When creating an object and inserting it into the tree, we verify that it does not already exist based on its spouse ID. This is accomplished by overriding the `equals(o)` method:

```java
@Override
public boolean equals(Object obj) {
    GenObj aux = (GenObj) obj;
    return nombre.equals(aux.nombre) && conyuge.equals(aux.conyuge);
}
```

## Graphical User Interface (GUI)

The GUI allows you to create, modify, insert, delete, save, and open files. It also provides visualization of attributes for each node.

>You can download sample trees from the [examples folder](https://github.com/crixodia/nary-family-tree/tree/master/examples) and open them to test the functionality.
{: .prompt-tip }

![Graphical user interface](https://github.com/crixodia/nary-family-tree/raw/master/assets/gui.jpg)
_Graphical user interface_

## Download the Entire Project

If you're interested in exploring the entire project, you can download it from [GitHub](https://github.com/crixodia/nary-family-tree). Feel free to leave your comments and feedback!
