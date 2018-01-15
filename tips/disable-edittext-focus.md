# How to disable EditText from stealing the focus
EditText view has a tendency to always keep the focus, even after touching outside it.  
It also gets focus on activity start, this means that if it's not handled than when the activity starts the focus is at the EditText and the keyboard is opened.  
There are a few steps to the solution, you can implement all or just a part of them depending on the requirement.
## Close keyboard + lose focus on touch outside
- Applies to **all** EditText views in the activity.
- Add `dispatchTouchEvent` method to the relevant activity:
```java
@Override
public boolean dispatchTouchEvent(MotionEvent event) {
   if (event.getAction() == MotionEvent.ACTION_DOWN) {
       View v = getCurrentFocus();
       if (v instanceof EditText) {
           Rect outRect = new Rect();
           v.getGlobalVisibleRect(outRect);
           if (!outRect.contains((int) event.getRawX(), (int) event.getRawY())) {
               v.clearFocus();
               InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
               if (imm != null) {
                   imm.hideSoftInputFromWindow(v.getWindowToken(), 0);
               }
           }
       }
   }
   return super.dispatchTouchEvent(event);
}
```
(credit: https://gist.github.com/sc0rch/7c982999e5821e6338c25390f50d2993)
## Disable auto focus on activity start 
- Applies only to EditText views under the parent.
- Wrap the EditText in a ViewGroup (LinearLayout for example,) if it's not already nested.
- Add the following properties to the parent:
```r
android:focusable="true"
android:focusableInTouchMode="true"
```
## Disable focus + opened keyboard
- The previous step takes care of the problem for specific EditText views, you might not need this step.
- Applies to **all** EditText views in the app.
- Add to `AndroidManifest.xml` file:
```r
android:windowSoftInputMode="stateHidden"
```
