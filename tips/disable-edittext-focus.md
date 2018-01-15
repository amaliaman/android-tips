# How to disable EditText from stealing the focus
EditText view has a tendency to always keep the focus, even after touching outside it.

It also gets focus on activity start, this means that if it's not handled than when the activity starts the focus is at the EditText and the keyboard is opened.

There are a few stages to the solution, you can implement all or just a part of them depending on the requirement.
## Close keyboard + lose focus on touch outside
Add `dispatchTouchEvent` method to the relevant activity:
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
