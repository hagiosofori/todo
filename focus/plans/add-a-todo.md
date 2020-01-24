[feature: add a todo]()

**user interface and user experience design**

- [link to user interface design]()

---


**engineering & implementation plan**

TYPE DEFINITIONS
- Task
  - name: String
  - 

- ScheduledTask
  - task: Task
  - 

- TimeRange
  - startTime: int
  - endTime: int

---
CLASS DEFINITIONS


--- 
COMPONENTS & LOGIC : 

AddNewTaskView component
  - state
    - baseTask: Task || null
    - taskName: String
    - priority: Int
    - urgency: Int
    - currentScreen: int
    - tasks : [Task]
    - parent : Task || null

  - render the `Next`/`Done` button

---
BaseTemplateView
  - props
    - currentScreen
    - tasks

  - `fn getFilterTasks(filter: string) => tasks.map(each => each.name.contains(filter))`
```
render fn
---
{currentScreen === 1 && (
  <div>
    <h3>Would you like to use an existing task as a template?</h3>

    <button>No</button>
    <button onClick={toggleShowTasksDropdown}>Yes</button>
    {showTasksDropdown && 
    <select onChange={value => setBaseTask(value)}>
      {getFilteredTasks().map(each => (
        <option>{each.name}</option>
        ))}
    </select>}
  </div>
)}

```

---
FLOW :

Create New Task
```

```
- heading -> 'Create New Task'
- each of these appears in its own screen. carousel style. horizontal scroll.

- dropdown input -> 'Would you like to use an existing task as a template?'
  - clicking on this shows a dropdown containing the names of all the tasks you've created already

  - default -> none

  - selecting a base template causes all the fields to be prepopulated with the details of that task.


- Text input -> name of task
  - validation -> escape the input

- importance of task
  - text -> 'How important is this task to you?'
  - 5 exclamation marks.
    - selecting the first means lowest priority... and so on to the highest priority, the fifth exclamation mark.
    - 1 -> 'least important'
    - 2 -> 'a bit important'
    - 3 -> 'quite important'
    - 4 -> 'very important'
    - 5 -> 'my life depends on this'

- urgency of task
  - text -> 'How soon do you have to complete this?'
  - options:
    - today
    - in [x] days' time
    - in [x] weeks' time
    - in [x] months' time
    - in [x] years' time

- preferred time range
  - text -> `Is there a particular time window you'd prefer to complete this task in?`

  - `Yes` & `No` buttons

  - on selecting `No`, slide to next screen.

  - on selecting 'Yes', start time and end time selectors appear. minutes and hours.
    - time is stored as a number between 0 and 1440.
    - conversion algorithm -> `(hours * 60) + minutes`

  - if is a subtask, preferred time range can't be before start time or after end time of parent task's time range.

- is habit?
  - if this is a subtask, and its parent isn't a habit, it can't be a habit. skip this step.

  - text -> `Is this task a habit?`

  - options -> `Yes`, `No`

  - on selecting `Yes`, show regularity options
    - weekly
      - on selecting weekly, days of the week show up. user selects which days of the week this task should show up.
    - monthly
      - on selecting monthly, days of the month show up. user selects the days of the month on which this task should show up
    - yearly
      - on selecting yearly, date picker shows up. user selects the days of the year on which this task should show up.
      - user can 'add another day' to the year, to show new date picker for the year.

- labels
  - text -> `Add labels to your task`
  - 
  - if this is a subtask, all the parent's labels are automatically applied.

  - all currently existing labels are shown on screen.
    - labels have their respective background colors

  - filter input can filter them out, to make it easier to find a label.

  - one of the labels has text `Create new label`. tapping that label shows a modal, 
    - modal has a field for label name, and a field for label color.
    - on creating the new label, it is automatically added to the labels for the task being created.

  - on this page, instead of `Next`, we'll have `Done`. clicking on `Done` saves the values, shows a success message, and takes you to the home page.
