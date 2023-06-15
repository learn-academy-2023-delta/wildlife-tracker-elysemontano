# Rails API
API (Application Programming Interface) - responsible for transmitting information across the internet as JSON

JSON (Javascript Object Notation) - data structure that is supported by most programming languages, modeled off of Javascript

## Rails Generate Commands
- Model: migration file and model file
- Controller: controller, view associated with controller
- Migration: migration file generated to make changes to schema

- Resource: model, controller, views, routes, respective rspec files
`$ rails g resource Student name:string cohort:string`
  We are given all of our RESTful routes (GET, POST, PUT, DELETE)

/config/routes.rb
  ```ruby
Rails.application.routes.draw do
  resources :students
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html

  # Defines the root path route ("/")
  # root "articles#index"
end

  ```

## RESTful Routes
- Show: show one instance
- Index: get's all instances
- New: form(view)
- Create: adds new instance to database
- Update: updates the instance in the database
- Edit: form(view)
- Destroy: deletes an instance

For API:
- Index
- Show
- Destroy
- Create
- Update

## Postman - GUI to display data from API
- Create a new tab
- Run server `$ rails s`

### Index
- Create index method
```ruby
  def index
    students = Student.all 
    render json: students
  end
```
- Set a GET request to localhost:3000/students
- Set view to JSON

```javascript
[
    {
        "id": 1,
        "name": "Jesus",
        "cohort": "Delta",
        "created_at": "2023-06-15T20:29:39.375Z",
        "updated_at": "2023-06-15T20:29:39.375Z"
    },
    {
        "id": 2,
        "name": "Brandon",
        "cohort": "Delta",
        "created_at": "2023-06-15T20:29:51.921Z",
        "updated_at": "2023-06-15T20:29:51.921Z"
    },
    {
        "id": 3,
        "name": "Ricky",
        "cohort": "Delta",
        "created_at": "2023-06-15T20:30:03.030Z",
        "updated_at": "2023-06-15T20:30:03.030Z"
    },
    {
        "id": 4,
        "name": "Padge",
        "cohort": "Delta",
        "created_at": "2023-06-15T20:30:20.118Z",
        "updated_at": "2023-06-15T20:30:20.118Z"
    }
]
```

### Show
- Created show method
```ruby
  def show
    student = Student.find(params[:id])
    render json: student
  end
```
- GET request to localhost:3000/students/1

```javascript
{
    "id": 1,
    "name": "Jesus",
    "cohort": "Delta",
    "created_at": "2023-06-15T20:29:39.375Z",
    "updated_at": "2023-06-15T20:29:39.375Z"
}
```

### Create
- Create the create method
```ruby
  def create
    student = Student.create(student_params)
    if student.valid?
      render json: student 
    else
      render json: student.errors
    end
  end

  private
  def student_params
    params.require(:student).permit(:name, :cohort)
  end
```

- Select in Postman: Body, Raw, Text -> JSON
- Disable Authenticity Token while in development so Postman can submit data to our rails database

app/controllers/application_controller.rb
```ruby
class ApplicationController < ActionController::Base
  skip_before_action :verify_authenticity_token
end

```

```javascript
{
    "id": 5,
    "name": "Miguel",
    "cohort": "Delta",
    "created_at": "2023-06-15T21:19:05.133Z",
    "updated_at": "2023-06-15T21:19:05.133Z"
}
```

## Update
- Create the update method
```ruby
  def update
    student = Student.find(params[:id])
    student.update(student_params)
    if student.valid?
      render json: student 
    else
      render json: student.errors
    end
  end
```
- Set URL to localhost:3000/students/5
- Change to PATCH request
- Update params


```javascript
{
    "name": "Miguel",
    "cohort": "Delta 2023",
    "id": 5,
    "created_at": "2023-06-15T21:19:05.133Z",
    "updated_at": "2023-06-15T21:26:35.008Z"
}
```

### Destroy
- Create the destroy method
```ruby
  def destroy
    student = Student.find(params[:id])
    if student.destroy 
      render json: student 
    else
      render json: student.errors
    end
  end
```

- Change request to delete
- Change URL to localhost:3000/students/2


```javascript
{
    "id": 2,
    "name": "Brandon",
    "cohort": "Delta",
    "created_at": "2023-06-15T20:29:51.921Z",
    "updated_at": "2023-06-15T20:29:51.921Z"
}
```

To check that the instance was removed:
- Change request to GET
- Change URL to localhost:3000/students

```javascript
[
    {
        "id": 1,
        "name": "Jesus",
        "cohort": "Delta",
        "created_at": "2023-06-15T20:29:39.375Z",
        "updated_at": "2023-06-15T20:29:39.375Z"
    },
    {
        "id": 3,
        "name": "Ricky",
        "cohort": "Delta",
        "created_at": "2023-06-15T20:30:03.030Z",
        "updated_at": "2023-06-15T20:30:03.030Z"
    },
    {
        "id": 4,
        "name": "Padge",
        "cohort": "Delta",
        "created_at": "2023-06-15T20:30:20.118Z",
        "updated_at": "2023-06-15T20:30:20.118Z"
    },
    {
        "id": 5,
        "name": "Miguel",
        "cohort": "Delta 2023",
        "created_at": "2023-06-15T21:19:05.133Z",
        "updated_at": "2023-06-15T21:26:35.008Z"
    }
]
```