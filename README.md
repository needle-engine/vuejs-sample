# Needle Engine Vuejs Sample

This sample shows how to add Needle Engine to your vuejs based web project.   

- Add the `<needle-engine>` web component to any part of your vuejs component. See for example [`App.vue`](https://github.com/needle-engine/vuejs-sample/blob/183dfcc96ae3516b07ce952dc2494aaebb44cf44/src/App.vue#L26)
- Additionally the [`SlideDeck.vue`](https://github.com/needle-engine/vuejs-sample/blob/main/src/components/SlideDeck.vue) file shows how to communicate from Vue components with any script or component in your Needle Engine scene by accessing the animator component and updating it's state and networking the currently active slide using Needle Engine's networking features.

See the sample [live here](https://engine.needle.tools/samples/vue.js-integration/?overlay=samples)


https://github.com/needle-engine/vuejs-sample/assets/5083203/34e0b327-ad0c-48ca-97fb-f4a40d075ada


The project is even networked:    

https://github.com/needle-engine/vuejs-sample/assets/5083203/07bacc51-41d9-4620-a963-5aa9d6e059a4



# A look at the Unity scene

In Unity the animation's are setup using an simple animator controller.  
From vuejs we simply set the `slide` index on the Animator.
![image](https://github.com/needle-engine/vuejs-sample/assets/5083203/d2bb7ae6-f2e1-42f9-97c2-dab53661b492)


https://github.com/needle-engine/vuejs-sample/assets/5083203/a2ccaf68-b037-4cf2-be60-3f7b24bbc9a4



# Links

[needle.tools](https://needle.tools) - [samples](https://engine.needle.tools/samples)
