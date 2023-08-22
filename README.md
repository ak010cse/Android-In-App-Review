# Android-In-App-Review

A sample demonstration of Android's new In-App Review API which lets you prompt users to submit Play Store ratings and reviews without the inconvenience of leaving your app or game.

# HOW TO USE

1. Import the Play Core Library into your project: Add the following in your app’s build.gradle file:

```
// In your app’s build.gradle file:
...
dependencies {
    // This dependency is downloaded from the Google’s Maven repository.
    // So, make sure you also include that repository in your project's build.gradle file.
    implementation 'com.google.android.play:review:2.0.1'

    // For Kotlin users also add the Kotlin extensions library for Play In-App Review:
    implementation 'com.google.android.play:review-ktx:2.0.1'
    ...
}
```

2. Create the ReviewManager instance

```
// If using kotlin: 
val manager = ReviewManagerFactory.create(context)

// If using Java:
ReviewManager manager = ReviewManagerFactory.create(context)
```

3. Request a ReviewInfo object
```
// If using kotlin: 
val request = manager.requestReviewFlow()
request.addOnCompleteListener { request ->
    if (request.isSuccessful) {
        // We got the ReviewInfo object
        val reviewInfo = request.result
        val flow = manager.launchReviewFlow(activity, reviewInfo)
        flow.addOnCompleteListener { _ ->
            // The flow has finished. The API does not indicate whether the user
            // reviewed or not, or even whether the review dialog was shown. Thus, no
            // matter the result, we continue our app flow.
        }
    } else {
        // There was some problem, continue regardless of the result.
        // you can show your own rate dialog alert and redirect user to your app page
        // on play store.
    }
}

// If using Java
ReviewManager manager = ReviewManagerFactory.create(this);
Task<ReviewInfo> request = manager.requestReviewFlow();
request.addOnCompleteListener(task -> {
    if (task.isSuccessful()) {
        // We can get the ReviewInfo object
        ReviewInfo reviewInfo = task.getResult();
        Task<Void> flow = manager.launchReviewFlow(activity, reviewInfo);
        flow.addOnCompleteListener(task -> {
            // The flow has finished. The API does not indicate whether the user
            // reviewed or not, or even whether the review dialog was shown. Thus, no
            // matter the result, we continue our app flow.
        });
    } else {
        // There was some problem, continue regardless of the result.
        // you can show your own rate dialog alert and redirect user to your app page
        // on play store.
    }
});
```

For more detailed information please follow this link: https://developer.android.com/guide/playcore/in-app-review

![iar-flow](https://github.com/ak010cse/Android-In-App-Review/assets/41910370/e39ef3a9-601b-4a65-9598-b63097d8c366)
