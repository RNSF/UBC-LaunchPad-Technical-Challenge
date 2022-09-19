# UBC-LaunchPad-Technical-Challenge

=== PROBLEM ===

iOS: 
You're fed up with all the rain this winter, and you decide to write an app that alerts you when it is about to rain. You know that you could just check the weather, but it would be great if the app reminded you ahead of time. How would you build such an app? What are the biggest challenges you foresee when making this rain alert app? 

----------------------------------------------------
----------------------------------------------------

=== SOLUTION ===

Design:
There are a few key design decisions that need to be made for the rain alert app:

--------------------------

Problem: How soon before rainfall should users be alerted?
1. Choosing a period which is too short may leave the user in a position where they are out and don’t have easy access to rain gear. In this scenario, it would have been better if the user was notified earlier.
2. Choosing a period which is too long can lead to the user forgetting about the reminder, and cause the reminder to be inaccurate to changing weather data.

Solutions: 
1.  The period of 1 day seems to offer a nice balance between being too long and too short.
2. In addition, one feature of the app could allow users to adjust this metric. For example, choosing to be warned 3 hours, 6 hours, 12 hours, or 24 hours before rainfall.
3. The user could also instead pick certain times of the day in which they would like to be reminded, for example every morning, as this allows them to prepare for the day ahead. This could be implemented with a simple time picker.

--------------------------

Problem: What constitutes rainfall that a user needs to be warned about?
1. Precipitation levels are often graphed percentage wise, showing what percentage that a certain area will get rain. At what percentage is necessary time to notify users?

Solutions
1. A solution to this will be to notify users of the probability in the notification that it might rain. For example: “🌧️ Rain alert! 25% change of rain at 7:00pm 🌧️”
2. There could also be a lower threshold (which the user could edit in the app) where the user wouldn’t be notified, say 15%.

--------------------------

Problem: How will the app know where the user is located?

Solutions:
1. The user could be prompted to manually set their location. This would then not require the user’s device to use GPS functionality which can be costly on energy usage. This may be an issue due to the use of background processes in the app (discussed more in the technical section).
2. The app could also get the GPS location of the user, and instead use that to retrieve weather data. This solution is much more seamless, and is good for users who travel frequently.

--------------------------

Overall the in-app features should include:
1. Home-page:
  a) A home-page for viewing the time until the next rainfall/when the next rainfall will be, or a separate message if there is no chance of rain.
  b) A count-down clearly showing when the user will next be notified of the upcoming rainfall.
  c) Option buttons to edit options.
2. Option-page:
  a) Choose what percentage change of precipitation the user wants to be notified about.
  b) Choose how to be notified, e.g. every day at a certain time, or a certain amount of time before rainfall starts.

An example notification that the user receives from the app may look like:
“🌧️ Rain alert! 25% change of rain at 7:00pm 🌧️”

----------------------------------------------------
----------------------------------------------------

Technical:
Problem: How will weather data be accessed?

Solutions: 
1. This can be easily done by using an online weather API, like OpenWeather (https://openweathermap.org/api/one-call-3).
2. However, a better solution may be to use Apple’s recently released WeatherKit (https://developer.apple.com/weatherkit/ ), since it can more easily be added to iOS apps.

--------------------------

Problem: How will the user be notified?

Solution:
1. We can set up a background task which calls the WeatherKit API. Since whether backgrounds tasks will run is dependent on a variety of factors, such as if the phone is in lower power mode, or how energy dependant the background task is, we will try to make the app not super dependent on using a background task multiple times a day. This can be done by scheduling the next notification whenever the background task is called, even if it is not for multiple hours. Of course, if weather data were to change in that time this could be problematic, so we would want to ensure that as many background tasks are run as possible by limiting the energy and data usage. This factor is the only factor that app designers really have control over to determine how often their background task is run (see https://developer.apple.com/videos/play/wwdc2020/10063/)). Relying not as heavily on short term data, will also help if the user loses internet connection during part of the day.

--------------------------

Problem: How will the app remain energy and data usage efficient? 

Solution:
1. The most energy expensive part of the app would probably be accessing the GPS to get the location of the user. One solution is to call the GPS more often, only if the user moves around a lot (which can be determined by their previous GPS data). For example, if the user was touring Europe for 3 weeks, then the GPS would be called more frequently than if the user were at home working for 6 months.

--------------------------
