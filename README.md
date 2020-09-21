# Todoist

Endpoint : https://in-elephant-10.hasura.app/v1/graphql

## User

#### Fields -

- username [text, unique]
- name [text]
- email [Unique]
- bio [text, nullable]
- avatar [text, nullable]
- password [text]
- user_id [text, primary key, unique]
- created_at [timestamp with time zone, default: now()]
- updated_at [timestamp with time zone, default: now()]

### Create User -

```

mutation CreateUser {
  insert_users(objects: {avatar: "", bio: "", email: "piyush@gmail.com", name: "Piyush Sinha", password: "12345", username: "piyush", user_id: "12345"}) {
    returning {
      avatar
      bio
      created_at
      email
      name
      password
      updated_at
      user_id
      username
    }
  }
}

```

### Update user -

```

mutation UpdateUser {
  update_users(_set: {username: "piyush07", name: "Piyush"}, where: {username: {_eq: "piyush"}}) {
    returning {
      avatar
      bio
      created_at
      email
      name
      password
      updated_at
      user_id
      username
    }
  }
}

```

### Delete user -

```
mutation DeleteUser {
  delete_users(where: {username: {_eq: "piyush07"}}) {
    returning {
      avatar
      bio
      created_at
      email
      name
      password
      updated_at
      user_id
      username
    }
  }
}

```

### User's Profile

```
query UserProfile {
  users(where: {username: {_eq: "sanjib"}}) {
    avatar
    bio
    created_at
    email
    name
    password
    updated_at
    user_id
    username
    projects {
      project {
        project_id
        title
        created_at
        updated_at
      }
    }
  }
}

```

## Project (Todo list) -

#### Fields

- title [text]
- project_id [text, primary key, unique]
- admin [text]
- created_at [timestamp with time zone, default: now()]
- updated_at [timestamp with time zone, default: now()]

### Create Project

```
mutation CreateProject {
  insert_projects(objects: {admin: "u1", title: "Todoist", project_id: "p001"}) {
    returning {
      project_id
      created_at
      title
      updated_at
      admin
    }
  }
}

mutation AddMember {
  insert_members(objects: {member_id: "m001", project_id: "p001", user_id: "u1"}) {
    returning {
      project_id
      user_id
    }
  }
}

```

### Update Project -

```
mutation UpdateProject {
  update_projects(_set: {title: "Todoist360"}, where: {project_id: {_eq: "p001"}}) {
    returning {
      project_id
      title
      admin
      created_at
      updated_at
    }
  }
}

```

### Project Informations

```
query ProjectInfo {
  projects(where: {project_id: {_eq: "p001"}}) {
    project_id
    title
    admin
    created_at
    updated_at
    tasks {
      task_id
      title
      is_completed
      author {
        user_id
        name
        username
        email
        bio
        avatar
        created_at
        updated_at
      }
      assigned_to {
        user_id
        name
        username
        email
        bio
        avatar
        created_at
        updated_at
      }
      priority
      scheduled_at
      created_at
      updated_at
    }

    comments {
      comment_id
      text
      author
      created_at
      updated_at
    }

    members {
      user {
        user_id
        name
        username
        email
        bio
        created_at
        updated_at
        password
      }
    }
  }
}


```

### All Projects

```
query ProjectInfo {
  projects {
    project_id
    title
    admin
    created_at
    updated_at
    tasks {

      task_id
      author {
        user_id
        name
        username
        email
        bio
        avatar
        created_at
        updated_at
      }
      title
      is_completed
      assigned_to {
        user_id
        name
        username
        email
        bio
        avatar
        created_at
        updated_at
      }
      priority
      scheduled_at
      created_at
      updated_at
    }
    comments {

      comment_id
      text
      author
      created_at
      updated_at
    }
    members {
      user {
        user_id
        name
        username
        email
        bio
        created_at
        updated_at
        password
      }
    }
  }
}


```

### Delete Projects

```
mutation DeleteProject {
  delete_projects(where: {project_id: {_eq: "p002"}}) {
    returning {
      admin
      project_id
      title
      created_at
      updated_at
    }
  }
}

mutation RemoveMembersInfo {
  delete_members(where: {project_id: {_eq: "p002"}}) {
    returning {
      project_id
      user_id
    }
  }
}

```

## Task

#### Fields

- title [text]
- project_id [text, primary key, unique]
- user [text]
- created_at [timestamp with time zone, default: now()]
- updated_at [timestamp with time zone, default: now()]
- scheduled_at [timestamp with time zone, nullable]
- assigned_user [text, nullable]
- priority [ integer, default: 0]

### Add Task

```
mutation AddTask {
  insert_tasks(objects: {author_id: "12346", is_completed: false, project_id: "p001", task_id: "t006", title: "Task #6", assigned_user: "12345", priority: 4}) {
    returning {
      task_id
      title
      is_completed
      project_id
      author {
        user_id
        username
        name
        email
        bio
        avatar
      }
      assigned_to {
        user_id
        username
        name
        email
        bio
        avatar
        created_at
        updated_at
      }
      priority
      scheduled_at
      created_at
      updated_at
    }
  }
}

```

### Update Task

```
mutation UpdateTask {
  update_tasks(_set: {is_completed: true}, where: {task_id: {_eq: "t001"}}) {
    returning {
      author_id
      task_id
      project_id
      title
      is_completed
      created_at
      updated_at
    }
  }
}

```

### Assign task later

```
mutation AssignTask {
  update_tasks(where: {task_id: {_eq: "t001"}}, _set: {assigned_user: "12345"}) {
    returning {
      task_id
      title
      is_completed
      priority
      scheduled_at

      assigned_to {
        user_id
        name
        email
        username
        bio
        avatar
        password
        created_at
        updated_at
      }
      author {
        user_id
        name
        email
        username
        bio
        avatar
        password
        created_at
        updated_at
      }
    }
  }
}

```

### Delete Task

```
mutation DeleteTask {
  delete_tasks(where: {task_id: {_eq: "t002"}}) {
    returning {
      author_id
      task_id
      project_id
      title
      is_completed
      created_at
      updated_at
    }
  }
}

```

### Task Info

```
query TaskInfo {
  tasks(where: {task_id: {_eq: "t006"}}) {
    task_id
    title
    is_completed
    project_id
    author {
      user_id
      username
      name
      email
      bio
      avatar
    }
    assigned_to {
      user_id
      username
      name
      email
      bio
      avatar
      created_at
      updated_at
    }
    priority
    scheduled_at
    created_at
    updated_at
  }
}

```

### All tasks with specific priority

```
query TaskInfo {
  tasks(where: {priority: {_eq: 4}}) {
    task_id
    title
    is_completed
    project_id
    author {
      user_id
      username
      name
      email
      bio
      avatar
    }
    assigned_to {
      user_id
      username
      name
      email
      bio
      avatar
      created_at
      updated_at
    }
    priority
    scheduled_at
    created_at
    updated_at
  }
}

```

### All tasks of a particular project

```
query AllTasks {
  tasks(where: {project_id: {_eq: "p001"}}) {
      author_id
      task_id
      project_id
      title
      is_completed
      created_at
      updated_at
  }
}

```

### All my tasks

```
query MyAllTasks {
  tasks(where: {assigned_to: {user_id: {_eq: "12345"}}}) {
    task_id
    title
    is_completed
    author {
      name
      email
      password
      bio
      avatar
      created_at
      updated_at
      user_id
      username
    }
    assigned_to {
      name
      email
      created_at
      bio
      avatar
      password
      username
      user_id
      updated_at
    }
    priority
    scheduled_at
    project {
      title
      project_id
      created_at
      updated_at
      admin
    }
  }
}

```

## Comments

#### Fields -

- text [text]
- author [text]
- project_id [text]
- comment_id [text, primary key, unique]
- created_at [timestamp with time zone, default: now()]
- updated_at [timestamp with time zone, default: now()]

### Add Comments

```
mutation AddComments {
  insert_comments(objects: {author: "u1", comment_id: "c001", project_id: "p001", text: "Comment #1"}) {
    returning {
      author
      comment_id
      project_id
      text
      created_at
      updated_at
    }
  }
}

```

### Edit Comment

```
mutation EditComment {
  update_comments(_set: {text: "Comment #01"}, where: {comment_id: {_eq: "c001"}}) {
    returning {
      author
      comment_id
      project_id
      text
      created_at
      updated_at
    }
  }
}

```

### Comment Information

```
query CommentInfo {
  comments(where: {comment_id: {_eq: "c001"}}) {
    author
    comment_id
    text
    updated_at
    created_at
    project {
      admin
      title
      project_id
      members {
        user {
          name
          email
          bio
          avatar
          username
        }
      }
      created_at
      updated_at
      tasks {
        task_id
        title
        is_completed
        author_id
        created_at
        updated_at
      }
    }
  }
}

```

### All Comments of a particular project

```
query CommentInfo {
  comments(where: {project_id: {_eq: "p001"}}) {
    author
    comment_id
    text
    updated_at
    created_at
    project {
      admin
      title
      project_id
      members {
        user {
          name
          email
          bio
          avatar
          username
        }
      }
      created_at
      updated_at
      tasks {
        task_id
        title
        is_completed
        author_id
        created_at
        updated_at
      }
    }
  }
}

```

### Delete Comment

```
mutation DeleteComment {
  delete_comments(where: {comment_id: {_eq: "c002"}}) {
    returning {
      author
      comment_id
      project_id
      text
      created_at
      updated_at
    }
  }
}

```
