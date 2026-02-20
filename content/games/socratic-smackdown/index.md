---
title: "Socratic Smackdown"
summary: "A platform fighter where animalized version of famous philosophers duke it out rather than talk it out!"
weight: 10

type: "Games"
tags: ['Unity', 'C#']
featured: true
---


<div style="float: left; width: 100%; max-width: 550px; margin-right: 20px; margin-bottom: 50px;">
  <iframe 
    src="https://www.youtube.com/embed/TkTGDHL3t8U"
    allowfullscreen
    style="width: 100%; height: 338px;">
  </iframe>
</div>

# About This Game

Socratic Smackdown is a 2.5D platform fighter where you play as animalized versions of classic philosophers as they fight for a thought in a king-of-the-hill style game mode.

Do cool combos, use special philosophy-inspired moves, and send opponents flying off the stage in a fun-for-all fighting game experience!

<div style="float: right; width: 100%; max-width: 550px; margin-left: 100px; margin-bottom: 150px; position: relative;">
  <iframe 
    src="https://itch.io/embed/4274440?border_width=0&dark=true" 
    frameborder="0" 
    style="position: absolute; top: 0; left: 0; width: 100%; height: 165px;">
  </iframe>
</div>

<img src="SharkratesRender1.png" alt="My Image" width = "550" style="float: right; margin-left:20px; margin-bottom:20px;">

<br><br>

# Project Details

*Roles:* Lead Programmer, Systems & Tools Programmer, Repo Master

*Team Size:* 15

*Duration:* September 2025 - Present

*Technology:* Unity, C#

* Built using Scrum and Agile methodologies
* Pitched to Champlain College faculty & alumni and was chosen to be "Greenlit" to continue working in the following semester

<br>

# State Machine

Designed and implemented a character state machine system supporting multiple states, accurate state tracking, and smooth transitions between states.

Developed a reusable Base State template to streamline the creation of individual state classes and ensure consistent structure and behavior across all states.

## Base State

```csharp
// All states inherit this class
public abstract class BaseState : IState
{ 
    public abstract StateID stateID { get; }

    public virtual void EnterState(PlayerController context) { }
    public virtual void UpdateState(PlayerController context) { }
    public virtual void ExitState(PlayerController context) { }
}
```

## Transition

```csharp
public delegate bool Condition(PlayerController context);

public class Transition
{
    public StateID to;
    public Condition condition;

    public Transition(StateID to, Condition condition)
    {
        this.to = to;
        this.condition = condition;
    }
}
```

## State Machine

```csharp
public class StateMachine
{
    public BaseState currentState
    {
        get; private set;
    }

    public BaseState nextState
    {
        get; private set;
    }

    private Dictionary<StateID, BaseState> states = new();
    private Dictionary<StateID, List<Transition>> transitions = new();

    // Register states for each character's state machine
    public void RegisterState(BaseState state, PlayerController context)
    {
        states[state.stateID] = state;

        if(currentState == null)
        {
            currentState = state;
            currentState.EnterState(context);
        }
    }

    // Add transitions from one state to another
    public void AddTransition(StateID from, StateID to, Condition condition)
    {
        if(!transitions.ContainsKey(from))
        {
            transitions[from] = new List<Transition>();
        }

        transitions[from].Add(new Transition(to, condition));
    }

    // Update the state and check for transitions
    public void Update(PlayerController context)
    {
        if (currentState == null)
            return;

        if(transitions.TryGetValue(currentState.stateID, out var stateTransitions))
        {
            foreach (var transition in stateTransitions)
            {
                if(transition.condition(context))
                {
                    ChangeState(states[transition.to], context);
                    break;
                }
            }
        }

        currentState.UpdateState(context);
    }

    // Changes the state once a transition is valid
    public void ChangeState(BaseState newState, PlayerController context)
    {
        nextState = newState;

        currentState.ExitState(context);
        currentState = newState;

        currentState.EnterState(context);
    }
}
```