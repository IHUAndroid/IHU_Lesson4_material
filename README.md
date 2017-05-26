# IHU_Lesson4_material

### Recap:
In lesson 3, we processed the weather JSON by using the JSONOBject class and displayed the parsed real weather data in the mobile screen.

Today we will create a second screen (Activity) that will be displayed when the user taps on a list view item. i.e. on a specific day of the foreacast.


## Second Screen (Activity)

We will now add a second screen - activity which will present the weather data of a specific day that was tapped on the forecast list view. 

We will begin with a simple [Toast (like a popup message box)](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) to test the tapping function. We'll going to need an ```Item Click Listener```.

As you will see, we create the listener anonymously for code simplicity. So, with this temporary ```toaster``` solution, the correct function of the listener will be tested.

Initially, only this short portion of code is enough for the listener. This block displays a Toaster every time a list item is tapped:

```
 listView.setOnItemClickListener(new AdapterView.OnItemClickListener(){

            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                final String Item = adapter.getItem(position);
                //Intent intent = new Intent(getActivity(),)

                Toast toast = Toast.makeText(getActivity(), Item, Toast.LENGTH_LONG);
                toast.show();
            }
        });
```

Now that we are sure that tapping event actually works, lets move to the creation of the second Activity. 
Create a new class ```DetailActivity```



```
package com.example.android.sunshine.app;

import android.content.Intent;
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.support.v7.app.ActionBarActivity;
import android.view.LayoutInflater;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

public class DetailActivity extends ActionBarActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_detail);
        if (savedInstanceState == null) {
            getSupportFragmentManager().beginTransaction()
                    .add(R.id.container, new PlaceholderFragment())
                    .commit();
        }
    }


    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.detail, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            Intent intent = new Intent(this, SettingsActivity.class);
            startActivity(intent);
            return true;
        }

        return super.onOptionsItemSelected(item);
    }

    /**
     * A placeholder fragment containing a simple view.
     */
    public static class PlaceholderFragment extends Fragment {

        public PlaceholderFragment() {
        }

        @Override
        public View onCreateView(LayoutInflater inflater, ViewGroup container,
                                 Bundle savedInstanceState) {

            Intent intent = getActivity().getIntent();
            View rootView = inflater.inflate(R.layout.fragment_detail, container, false);

            if(intent != null && intent.hasExtra(Intent.EXTRA_TEXT)){
                String text = intent.getStringExtra(Intent.EXTRA_TEXT);
                TextView detailText = (TextView)rootView.findViewById(R.id.detail_text);
                detailText.setText(text);
            }
            return rootView;
        }
    }
}
```

And the fragment_detail.xml. Notice that Hierarhical parent is the MainActivity

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_detail"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.example.android.sunshine.app.DetailActivity">

<TextView
    android:id="@+id/detail_text"
    android:text=""
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />

</RelativeLayout>

```

## Communication between activities

## Intents

Intents form the main way of communication between activities and are divided in **Explicit** and **Implicit**
We will now focus on Explicit Intents

You can think of Intent as a small POST box that we use to send packages to other people via post office. 
We place the item inside the package, we close it and we write the delivery address on the package.

Upon deliver, the receiver gets the package, opens it and retrieves the item from inside. 

__This is the EXPLICIT Intent__

Below, follows the modification of ```setOnItemClickListener``` in order to send data to ```DetailActivity``` class via an Explicit Intent

```
 listView.setOnItemClickListener(new AdapterView.OnItemClickListener(){
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                final String item = mForecastAdapter.getItem(position);
                Intent intent = new Intent(getActivity(), DetailActivity.class);
                intent.putExtra(Intent.EXTRA_TEXT, item);
                startActivity(intent);

            }
        });

```
