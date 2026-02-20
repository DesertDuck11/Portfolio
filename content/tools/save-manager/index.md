---
title: "Save Manager"
summary: "A simple tool made for Unity 6 that allows the user to save and load data back from files with ease"
weight: 30

type: "Tools"
tags: ['Unity', 'C#', 'JSON']
featured: true
---

<div style="display: flex; flex-wrap: wrap; gap: 100px; margin-top: 0px">

  <div style="flex: 1 1 300px; min-width: 250px;">

# About This Project

Custom save file creation tool built for Unity 6

Features:

* Allows users to save and load data using keys
* Allows the creation of save files
* Allows the user to set where save files are stored
* JSON and Binary File Loading
* Creation of save states
* Ability to save non-traditional variables

  </div>

  <div style="float: right; min-width: 250px; width: 500px; max-width: 650px; margin-top: -20px; gap: 20px">

```csharp
using UnityEngine;

public class Example : MonoBehaviour
{
    [SerializeField] SaveData saveData;

    private void Awake()
    {
        SaveManager.Load<SaveData>("saveData");
    }

    private void OnDestroy()
    {
        SaveManager.Save("saveData", saveData);
    }

    [System.Serializable]
    private struct SaveData
    {
        public string playerName;
        public float health;
        public Vector3 position;
    }
}
```
  </div>
</div>

<br>

# Project Details 

*Team Size:* Solo

*Duration:* January 2026 - Present

*Technology:* Unity, C#, JSON
