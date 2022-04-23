# Scene Persist

### What is it?
In Unity, a version of the singleton pattern is often used to persist object between scenes. This isn't always necessary and can cause issues including essentially creating global state. Instead we can use a simple ScenePersist class to allow an object (and it's children) to persist between scenes.


### Implementation
1. Create an `ObjectSpawner` gameObject in your scene and attach the `PersistentObjectSpawner` script to it. 
2. Create a prefab called `PersistentObjects` whose children are all the objects you want to persist.
3. Child any objects that should persist to this prefab.

This class will spawn the `PersistentObjects` prefab unless that prefab has already spawned.

**PersistentObjectSpawner.cs**
```C#
    using UnityEngine;

    public class PersistentObjectSpawner : MonoBehaviour
    {
        // CONFIG DATA
        [Tooltip("This prefab will only be spawned once and persisted between " +
        "scenes.")]
        private GameObject persistentObjectPrefab = null;

        // PRIVATE STATE
        static bool hasSpawned = false;

        // PRIVATE
        private void Awake()
        {
            if (hasSpawned) return;

            SpawnPersistentObjects();

            hasSpawned = true;
        }

        private void SpawnPersistentObjects()
        {
            GameObject persistentObject = Instantiate(Resources.Load("Utility/PersistentObjects") as GameObject);
            DontDestroyOnLoad(persistentObject);
        }
    }

```
