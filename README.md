# 🚀 DesignPatterns


### Memento design pattern 🔖

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;We have an `Editor` class. Let's pretend we want to create an `undo mechanism`.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;We have a `content` field in the `Editor` class. For creating the undo mechanism,
we could make a `List of Strings` representing the previous contents.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;But what if later on, we add a new field named `title`? Now we have to
create another `List of Strings` representing the previous titles. This solution is
bad - <ins>it is not extensible.</ins>

![memento1](https://user-images.githubusercontent.com/31708451/162756839-22bbeddc-d802-4c9d-ba43-666fab09dfa8.png)



&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A better solution would be to create a new Class, named `EditorState` and keep here all
data of a state of the editor at a given time. In the `Editor` class, we will have
the fields we normally used, but instead of all those Lists, now we have a single
`List of type 'EditorState'` (representing all the previous states) -> <ins>aggregation
relationship</ins> between `Editor` and `EditorState`. However, this solution is <ins>violating
the `'Single Responsability Principle'`</ins>. (every class should have a
single responsability).

![memento2](https://user-images.githubusercontent.com/31708451/162757367-d77646cf-e740-4df6-88b4-f8fa4bddee40.png)



&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Here, our `Editor` class has two responsabilities:<br><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  ▶️ state management<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	▶️ providing the features we need from an editor

<br><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;We need to take all `state management work` outside this class and put it somewhere else.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;To do this, we can create a new class, named `History`, used for state management.
We are going to have a <ins>composition relationship</ins> between `History` and `EditorState`.
`History` class is going to store 0 or more `EditorState` objects in its list.
We also need 2 methods, `push(state)` and `pop()`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Back in the `Editor` class, we will create 2 new methods: `createState()` and `restore(state)`.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`createState()` stores the current state of the `Editor` in an `EditorState` object and returns it.
Then, we call the `push` method from `History` class to store this new state.
Now, between `Editor` and `EditorState` we have a <ins>dependency relationship</ins>, meaning that the
`Editor` class uses `EditorState`, because `createState()` returns an `EditorState` object.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The `restore(state)` method brings the `Editor` back to the passed state. It will reset its
fields, based on the state object.

![memento3](https://user-images.githubusercontent.com/31708451/162757629-c10807b8-1da3-41ef-9e36-da7f2a58424b.png)


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GOF book:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;▶️ Editor		    : Originator<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;▶️ EditorState 	: Memento<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;▶️ History		  : Caretaker<br>
