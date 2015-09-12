# lift-blank-heroku
The lift_blank 2.6 project dressed for Heroku deployment applied to InternBritany

[ ![Codeship Status for blmeadows/lift-blank-heroku](https://codeship.com/projects/9ea9c910-3b86-0133-aee8-428ee47fa127/status?branch=master)](https://codeship.com/projects/102116)

See it live at [http://internbritany.herokuapp.com/](http://internbritany.herokuapp.com/)

Or deploy your own by clicking this button:

[![Deploy to Heroku](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy)

## Applying to your Lift project
If you already have a Lift project that you want to deploy on Heroku, you will need to update the following.

1. `project/plugins.sbt`: Add `sbt-native-packager` and bump the `xsbt-web-plugin` to the latest
(that might be the trickier part if your project requires any customization.
It may be possible to deploy to Heroku without using the latest, but you will at least need to go to `0.9.0` for Jetty 9 to work).
2. `project/build.properties`: Bump to the latest sbt version to make sure `sbt-native-packager` works.
3. `build.sbt`: Bump Jetty to 9.
Enable `JettyPlugin` (from `xsbt-web-plugin`. Note that you will now start the server locally with `jetty:start`).
Enable `JavaAppPackaging` (for `sbt-native-packager`).
Add `bashScriptConfigLocation` setting, which will allow setting the Lift `run.mode` to `production`.
Add `mainClass` just in case there are other `main` methods in your project.
4. `Procfile`: Copy from this project.
Change `lift-2-6-heroku-starter-template` to match the `name` setting from `build.sbt` of your Lift project.
6. `src/main/scala/bootstrap/liftweb/Start.scala`: Copy from this project.
7. `src/test/scala/RunWebApp.scala`: This will no longer compile due to bumping to Jetty 9. Remove.

## Optional

1. `src/universal/conf/jvmopts`: Copy from this project.
Add any other JVM options you need for running your application in production.

## Testing locally
Before pushing to Heroku, you can verify your Heroku configuration locally.
Run `sbt stage`.
Execute the script specified in `Procfile` or run `$ heroku local`.

## Multiple Dynos
If you plan to run multiple web Dynos, you will need to [enable Session Affinity](https://blog.heroku.com/archives/2015/4/28/introducing_session_affinity).
