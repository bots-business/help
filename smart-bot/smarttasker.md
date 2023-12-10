# SmartTasker

## Introduction

`SmartTasker` is a comprehensive class designed for task management within bot applications. It offers a robust framework for handling tasks, tracking user progress, managing rewards, and more. This guide provides an overview of the `SmartTasker` class, explaining its core functionalities and how to effectively integrate it into your bot projects.

{% hint style="success" %}
We have bot demo: [BBDemoTaskBot](https://t.me/BBDemoTaskBot) - free available in the Store.&#x20;
{% endhint %}

### Key Features

* Task Management: Manages a list of tasks and user interactions with these tasks.
* Progress Tracking: Keeps track of user's completed and ongoing tasks.
* Reward System: Manages a balance system for rewarding users upon task completion.
* Dynamic Task Execution: Supports dynamic handling and execution of tasks based on user input and actions.

## Constructor

The constructor initializes the `SmartTasker` with necessary configurations.

#### Syntax:

```javascript
constructor(options);
```

#### Parameters:

* `options`: An object containing initial settings such as task list, balance, and a reference to `SmartBot`.

#### Example:

```javascript
let tasker = new SmartTasker({
  tasks: [...],
  balance: 100,
  smartBot: botInstance
});
```

## Core Methods

### getTasksForWork

Fetches tasks available for the user to work on.

**Usage:**

```javascript
let availableTasks = tasker.getTasksForWork();
```

### skipTask

Skips the current task and moves to the next one.

**Usage:**

```javascript
let hasNext = tasker.skipTask();
```

### defineTask

Defines the current task based on the task ID or task definition.

**Usage:**

```javascript
tasker.defineTask(taskIdOrDefinition);
```

### completeExecution

Marks the current task as completed and processes the reward.

**Usage:**

```javascript
tasker.completeExecution(taskId);
```

### addBalance

Adds a specified amount to the user's balance.

**Usage:**

```javascript
tasker.addBalance(amount);
```

### prepareTaskQuestion

Prepares a question related to a task for user interaction.

**Usage:**

```javascript
tasker.prepareTaskQuestion({ taskID: 'task1', onAnswer: 'handleAnswer' });
```

### acceptAnswer

Processes the user's answer to a task question.

**Usage:**

```javascript
let result = tasker.acceptAnswer(params);
```

###

## Best Practices

* **Consistency in Task Definitions**: Ensure that tasks are defined consistently and include all necessary information.
* **Error Handling**: Utilize the built-in error handling capabilities of `SmartTasker` to manage exceptions and provide feedback to users.
* **Integration with SmartBot**: Leverage the integration with `SmartBot` for a seamless user experience.
* **Task Progress Persistence**: Implement persistence mechanisms to save user progress and task completions.

