---
layout: post
title:      "Final Project- Todo App"
date:       2020-02-06 22:53:58 +0000
permalink:  final_project-_todo_app
---

Wow!
I can not believe how far I have got. I completed my final Project 

I found this project to be challenging but fun to create. It really showed me how much I love to code and how rewrding it feels to see your hard work displayed in the final product, My To Do app. 

I started off with my backend. I created the index, create, update and delete routes and the controller methods for each and rendered the methods in json in the controller. I set up te database to have a place to store all your Todos. I set up my backend and frontend on diffren servers.  With the backend set up, I was able to continue on to the frontend. 

Most of he client-side application was in the React-redux app. When you open up the application, there is three links your list, about, and help, located on the top left corner. I created these liks using the react router and by creating a component NavBar that named those links. I then imported the navbar component to the index. js file and then defined the routes using components as routes. 

import React from 'react';
import { NavLink } from 'react-router-dom';

const NavBar = () => {
  return (
    <div className= "navbar"> 
    <NavLink to="/">Your List </NavLink><br></br>
     <NavLink to="/about">About</NavLink><br></br>
   <NavLink to ="/help"> Help </NavLink>
    </div>
  );
};

export default NavBar;

and in index.js: 
ReactDOM.render(
    <Provider store ={store}>
<Router>
  <div>
    <NavBar/>
        <Switch>
          <Route exact path='/' component={App} />
          <Route path="/about/" component={About} />
          <Route path= "/help/" component= {Help} />
        </Switch>
        </div>
      </Router>    
</Provider>
,document.getElementById('root'));

The main page, your list, contains the form to create a todo, a checkbox to checkoff a todo, and a button to delete a todo.  

The about page containes the information about the applicaton 

The Help page gives you all the instructions and contact information that you would need 

I created a form that allows you to put in a title and date. i first created a action that will make a request using axios to post the todo i created to the backend. I connected the create to the store i created in the index.js file using MapDispatchToProps function. 

 export function addTodo(id, title, date ) {
    return { type: ADD_TODO, id: id, title: title, date: date}
  }
  

  export const createTodo =( title, date ) => {
    console.log(title)
    console.log(date)
    return(dispatch) => {
    if (!(title === '')) {
		axios.post('http://localhost:3000/api/v1/todos', {todo: {title: title, date:date}})
		.then(response => {
			dispatch(addTodo(response.data.id, response.data.title, response.data.date))
		})
		.catch(error => console.log(error))      
		}    
  }
  }
	 and my form ia as such: 
	
import React, { Component } from 'react'
import { createTodo} from '../actions/todosActions'
import { connect } from 'react-redux'

class CreateTodo extends Component {
constructor(props){
super(props);
this.state = {
  title: '',
  date: ''
} 
};

handleSubmit = (event) => {
   event.preventDefault()
   this.props.addTodo({
   title: this.state.title, 
   date: this.state.date  })
   this.setState( { title: ' ', 
    date: ''})
   
 }

 handleChange = event => {
  this.setState({
    [event.target.name]: event.target.value
 });
}

render() {
return(

  <div className= "todoInput"> 
    
     <form onSubmit = {(event) => this.handleSubmit(event)} >
<div>
<h3> Add Todo</h3><br></br>
       <input className= "titleInput"
              type= "text"
              name ="title"
              placeholder ="Add a task"
              maxLength= "60"
              value = {this.state.title}
            onChange= {this.handleChange}
              /> <br></br>
       </div>
       <input className= "dateInput"
              type= "text"
              name="date"
              placeholder ="date of task"
              value={this.state.date}
              onChange = {this.handleChange}
              /> 
              <br></br>

              <button type="submit" className="btn btn-default">Add</button>

     </form>
  </div>
)}
}

const mapDispatchToProps = dispatch => ({
  addTodo: ({title, date})=> {
    console.log(title)
    console.log(date)
    dispatch(createTodo(title, date))}
})

export default connect(null, mapDispatchToProps)(CreateTodo);

after you create your todos, there is a list of them displayed on the your list page. On the list you can view, update and delete it as you would like. 

In a container i placed all the code responsible for handling the state and props
mport React, { Component } from 'react'
import { getTodos} from '../actions/todosActions'
import {deleteTodo} from '../actions/todosActions'
import {updateTodo} from '../actions/todosActions'
import {connect} from 'react-redux'
 import TodoList from "../components/TodoList"



class TodosContainer extends Component {
  
  

componentDidMount() {
    this.props.getTodos()
    }



    render() {
        return (
          <div className= "container">
            <h1> What has to be done  </h1>
              <TodoList todos ={this.props.todos} delete={this.props.delete} update = {this.props.update}/>
              </div> 
        )}
} 

const mapStateToProps = (state) => {
    return {
      todos: state.todos
 
    }
  }

  const mapDispatchToProps = dispatch => {
    return {
      getTodos: () => dispatch(getTodos()),
      delete: id => dispatch(deleteTodo(id)), 
      update: (id) =>{
      
         dispatch(updateTodo(id))}
    
    
    }
  }
  
  export default connect(mapStateToProps, mapDispatchToProps)(TodosContainer)
	
	in this container i implamented the gettodos, updatetodos, and deletetodos actions that i created
	 


export function loadTodos(todos) {
    return { type: LOAD_TODOS, todos: todos }
  }
  
  
  export function toggleTodo(id) {
    return { type: TOGGLE_TODO, index: id }
  }


  
  export function deleteTodos(id) {
    return { type: DELETE_TODO, id: id  }
  export const getTodos = () => {
    return(dispatch) => {
    axios.get('http://localhost:3000/api/v1/todos')
    .then(response => {
    dispatch(loadTodos(response.data));
    })
    .catch(error => console.log(error))
  }
}

  
export const  updateTodo = (params) => { 
  return(dispatch) => {
    axios.put(`http://localhost:3000/api/v1/todos/${params.id}`, {todo: {done: params.checked}})
    .then(() => {
    dispatch(toggleTodo(params.id))
    })
    .catch(error => console.log(error))      
}
}

 export const deleteTodo = (id) => {
   return(dispatch) => {
    axios.delete(`http://localhost:3000/api/v1/todos/${id}`)
    .then(response => {
      dispatch(deleteTodos(id))
    })
    .catch(error => console.log(error))
  }


the loadtodos request from the backend all the instances of todos and then it gets displayed. The components that took care of displaying the data were as such:

import React, {Component}  from 'react'
import Todo from '../components/Todo'


class TodoList extends Component {

    renderTodos = () => {
      const todos= Array.from(this.props.todos)
        return todos.map( (todo) =><Todo delete={this.props.delete} key={todo.id} todo={todo} id={todo.id} 
            updateTodo={this.props.update} />)
      }

render(){
 
   return ( 
  
        <div className = "list">
          <ul className = "todoList">
      {this.renderTodos()}
    </ul> 
    </div>
    )
}
}
export default TodoList 

import React, {Component} from 'react';

class Todo extends Component { 

  updateTodo = (e, id) => {
    console.log(id)
    console.log(e.target.checked)
        this.props.updateTodo({ id: id, checked: e.target.checked})
    }
 
 deleteTodo= (id) => {
      this.props.delete(id)
  }    
render(){
 const todo= this.props.todo 
    return ( 
    <div>
    <ul>
      <li className = "todo" key ={todo.id}>
      <input className="taskCheckbox" type="checkbox" name="checkbox"
                checked={todo.done} onChange={(e) => this.updateTodo(e, todo.id)} />  
    <p className="title"> <label className = "todoTitle" >{todo.title} </label> </p>
  <p className= "forDate"> For:    <label className= "todoDate">  {todo.date}</label> </p>
  <span> <button className = "deleteTask" onClick={() => this.deleteTodo(todo.id)}>Delete</button></span>
      </li> 
      </ul> 
    </div>
  )
    }

};


export default Todo;

If you realize, the component, todolist, is put on the render function of the container component so that doing so i was able to create all the actions as props using the mapDispatchToProps function in the todos contianer. 

so when you see: this.props.todos or this.props.delete, it is being passed as a prop from the parent to child component 

this is just a brief summary of my code for my app. I hope this can help anyone that might need a little help. 



I placed the conatiner componenet in the app.js component that is in charge of displaying and containing all the other containers. The app.js gets rendered in index.js 









