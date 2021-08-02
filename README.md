# TinyPotionExample
<script type='text/javascript' src='https://storage.ko-fi.com/cdn/widget/Widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Support Me on Ko-fi', '#29abe0', 'G2G148Y9A');kofiwidget2.draw();</script> 
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

            var action = _world.Actions;
            action.DrinkPotion(player,_potions.Healing());
            action.DrinkPotion(npc,_potions.Invincibility());
            action.DrinkPotion(army,_potions.Vitality());
            action.DrinkPotion(army,_potions.Death());

            _world.UndoLastAction();
            _world.Save();
            
            OpenSaveFile();
        }
```

generates this save data:

```json
[
  {
    "Identity": {
      "Name": "Elara the Carpenter"
    },
    "Stats": [
      {
        "Name": "Health",
        "Value": 100,
        "Max": 100
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
    ]
  },
]
```
Etc...

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
            {"Health", x => x.WithIncrease(0.2f), true}
        };

        public Potion Death() => new Potion("Death", "Kills you")
        {
            {"Dead", true}
        };
    }
```

If this helped you in some way and you'd like to buy me a coffee. You can find me over on https://ko-fi.com/jasonstorey

<script type='text/javascript' src='https://storage.ko-fi.com/cdn/widget/Widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Support Me on Ko-fi', '#29abe0', 'G2G148Y9A');kofiwidget2.draw();</script> 

