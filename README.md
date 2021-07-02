# TinyPotionExample
Simple Example of flags, stats, potions, world actions and persistence that supports undo

(Just keep in mind this is a thought exercise written in an hour, not a real robust system. In a practical example I would do this test driven and have a qualified code covered test suite, use it as an example of a possible approach to boundary design and thats it.)

It runs this game logic:



```cs
 public void Run()
        {
            var player = _actors.CreatePlayer();
            var npc = _actors.CreateNpc();
            var army = _actors.CreateArmy(5);
            
            _world.Add(player);
            _world.Add(npc);
            _world.Add(army);
            
            _world.Actions.DrinkPotion(player,_potions.Healing());
            _world.Actions.DrinkPotion(npc,_potions.Invincibility());
            _world.Actions.DrinkPotion(army,_potions.Vitality());
            _world.Actions.DrinkPotion(army,_potions.Death());

            _world.UndoLastAction();
            _world.Save();
            OpenSaveFile();
        }
```

generates this save data:

```json
[
  {
    "Stats": [
      {
        "Max": 100,
        "Value": 100,
        "Name": "Health"
      }
    ],
    "Flags": [
      {
        "Name": "Invincibility",
        "Status": false
      },
      {
        "Name": "Dead",
        "Status": false
      }
    ],
    "Identity": {
      "Name": "Jeff the Bard"
    }
  },
  {
    "Stats": [
      {
        "Max": 100,
        "Value": 0,
        "Name": "Health"
      }
    ],
    "Flags": [
      {
        "Name": "Invincibility",
        "Status": true
      },
      {
        "Name": "Dead",
        "Status": false
      }
    ],
    "Identity": {
      "Name": "Elara the Soldier"
    }
  },
  {
    "Stats": [
      {
        "Max": 120,
        "Value": 0,
        "Name": "Health"
      }
    ],
    "Flags": [
      {
        "Name": "Invincibility",
        "Status": false
      },
      {
        "Name": "Dead",
        "Status": false
      }
    ],
    "Identity": {
      "Name": "Jeff the Duck Fancier"
    }
  },
  {
    "Stats": [
      {
        "Max": 120,
        "Value": 0,
        "Name": "Health"
      }
    ],
    "Flags": [
      {
        "Name": "Invincibility",
        "Status": false
      },
      {
        "Name": "Dead",
        "Status": false
      }
    ],
    "Identity": {
      "Name": "Shegorath the Carpenter"
    }
  },
  {
    "Stats": [
      {
        "Max": 120,
        "Value": 0,
        "Name": "Health"
      }
    ],
    "Flags": [
      {
        "Name": "Invincibility",
        "Status": false
      },
      {
        "Name": "Dead",
        "Status": false
      }
    ],
    "Identity": {
      "Name": "Temptation the Cat Whisperer"
    }
  },
  {
    "Stats": [
      {
        "Max": 120,
        "Value": 0,
        "Name": "Health"
      }
    ],
    "Flags": [
      {
        "Name": "Invincibility",
        "Status": false
      },
      {
        "Name": "Dead",
        "Status": false
      }
    ],
    "Identity": {
      "Name": "Temptation the Blacksmith"
    }
  },
  {
    "Stats": [
      {
        "Max": 120,
        "Value": 0,
        "Name": "Health"
      }
    ],
    "Flags": [
      {
        "Name": "Invincibility",
        "Status": false
      },
      {
        "Name": "Dead",
        "Status": false
      }
    ],
    "Identity": {
      "Name": "Ergoth the Blacksmith"
    }
  }
]
```

To Create and Modify stats with potions, you can use a potion factory like:

```cs
    public class PotionFactory
    {
        public Potion Invincibility() => new Potion("Invincibility","Makes you invincible")
        {
            {"Invincibility", true}
        };

        public Potion Healing() => new Potion("Healing","Heals to full")
        {
            {"Health", x=>x.Max}
        };

        public Potion Vitality() => new Potion("Vitality","Increase Max Health by 20%")
        {
            {"Health", x => (int)(x.Max * 1.2f), true}
        };

        public Potion Death() => new Potion("Death", "Kills you")
        {
            {"Dead", true}
        };
    }
```

If this helped you in some way and you'd like to buy me a coffee. You can find me over on https://ko-fi.com/jasonstorey

