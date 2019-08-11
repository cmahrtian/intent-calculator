# Chris Mahr's Calculator
-----

This calculator was built using the [Instant Prototyping](https://cli.vuejs.org/guide/prototyping.html) that's available as part of version 3.x of the Vue CLI system. Before attempting to run in your local environment, ensure that you have the following downloaded:

## Vue CLI 3

If you have previously installed the Vue CLI, check what version you are running by typing `vue --version` into a terminal window. If the output is any version older than 3.0.0, download the latest version by running the following command.

```
npm install -g @vue/cli
```

Once the download finishes, run `vue --version` again to ensure that you've been upgraded to 3.0.0.

## Vue Global Add-On

As explained in the aforementioned hyperlink, an additional global add-on is required for rapid prototyping.

```
npm install -g @vue/cli-service-global
```

-----

## Running the Application

Once the aforementioned global add-on is installed, clone this repo and navigate to its root in a terminal window. In the root directory, run the following command:

```
vue serve Calculator.vue
```

You should get a message that the app is running locally on [port 8080](http://localhost:8080/).

-----

## Next Steps

With additional time, I would include logic to detect keydown events for all 19 buttons on the calculator.