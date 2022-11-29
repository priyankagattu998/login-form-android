# login-form-android
creating a login for android application 
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
 
    <!--text view for heading-->
    <TextView
        android:id="@+id/idTVHeader"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="30dp"
        android:gravity="center_horizontal"
        android:padding="5dp"
        android:text="Welcome to my world \n Login Form"
        android:textAlignment="center"
        android:textColor="@color/purple_700"
        android:textSize="18sp" />
 
    <!--edit text for user name-->
    <EditText
        android:id="@+id/idEdtUserName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/idTVHeader"
        android:layout_marginStart="10dp"
        android:layout_marginTop="50dp"
        android:layout_marginEnd="10dp"
        android:hint="Enter UserName"
        android:inputType="textEmailAddress" />
 
    <!--edit text for user password-->
    <EditText
        android:id="@+id/idEdtPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/idEdtUserName"
        android:layout_marginStart="10dp"
        android:layout_marginTop="20dp"
        android:layout_marginEnd="10dp"
        android:hint="Enter Password"
        android:inputType="textPassword" />
 
    <!--button to register our new user-->
    <Button
        android:id="@+id/idBtnLogin"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/idEdtPassword"
        android:layout_marginStart="10dp"
        android:layout_marginTop="20dp"
        android:layout_marginEnd="10dp"
        android:text="Login"
        android:textAllCaps="false" />
 
</RelativeLayout>
 <!---redirecting to again login page--->
 <?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".HomeActivity">
     
    <!--text view for displaying heading-->
    <TextView
        android:id="@+id/idTVHeader"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:gravity="center_horizontal"
        android:text="Welcome back again to my world"
        android:textAlignment="center"
        android:textColor="@color/purple_700"
        android:textSize="18sp" />
     
    <!--text view for displaying user name-->
    <TextView
        android:id="@+id/idTVUserName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/idTVHeader"
        android:layout_centerInParent="true"
        android:layout_marginTop="20dp"
        android:gravity="center_horizontal"
        android:text="UserName"
        android:textAlignment="center"
        android:textColor="@color/purple_700"
        android:textSize="25sp" />
 
    <!--button for making user log out-->
    <Button
        android:id="@+id/idBtnLogout"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/idTVUserName"
        android:layout_centerInParent="true"
        android:layout_marginTop="10dp"
        android:text="LogOut"
        android:textAllCaps="false" />
     
</RelativeLayout>


<!---home activity java file-->
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

import com.parse.ParseUser;

public class HomeActivity extends AppCompatActivity {

	// creating a variable
	// for our text view..
	private TextView userNameTV;
	
	// button for logout
	private Button logoutBtn;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_home);
		logoutBtn = findViewById(R.id.idBtnLogout);
		
		// initializing our variables
		userNameTV = findViewById(R.id.idTVUserName);
		
		// getting data from intent.
		String name = getIntent().getStringExtra("username");
		
		// setting data to our text view.
		userNameTV.setText(name);
		
		// initializing click listener for logout button
		logoutBtn.setOnClickListener(new View.OnClickListener() {
			@Override
			public void onClick(View v) {
				// calling a method to logout our user.
				ParseUser.logOutInBackground(e -> {
					if (e == null) {
						Toast.makeText(HomeActivity.this, "User Logged Out", Toast.LENGTH_SHORT).show();
						Intent i = new Intent(HomeActivity.this, MainActivity.class);
						startActivity(i);
						finish();
					}
				});
			}
		});
	}
}

<!--mainactivity java file-->

import android.content.Intent;
import android.os.Bundle;
import android.text.TextUtils;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
 
import androidx.appcompat.app.AppCompatActivity;
 
import com.parse.ParseUser;
 
public class MainActivity extends AppCompatActivity {
     
    // creating variables for our edit text and buttons.
    private EditText userNameEdt, passwordEdt;
    private Button loginBtn;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
         
        // initializing our edit text  and buttons.
        userNameEdt = findViewById(R.id.idEdtUserName);
        passwordEdt = findViewById(R.id.idEdtPassword);
        loginBtn = findViewById(R.id.idBtnLogin);
         
        // adding on click listener for our button.
        loginBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                 
                // on below line we are getting data from our edit text.
                String userName = userNameEdt.getText().toString();
                String password = passwordEdt.getText().toString();
                 
                // checking if the entered text is empty or not.
                if (TextUtils.isEmpty(userName) && TextUtils.isEmpty(password)) {
                    Toast.makeText(MainActivity.this, "Please enter user name and password", Toast.LENGTH_SHORT).show();
                }
                 
                // calling a method to login our user.
                loginUser(userName, password);
            }
        });
    }
 
    private void loginUser(String userName, String password) {
        // calling a method to login a user.
        ParseUser.logInInBackground(userName, password, (parseUser, e) -> {
            // after login checking if the user is null or not.
            if (parseUser != null) {
                // if the user is not null then we will display a toast message
                // with user login and passing that user to new activity.
                Toast.makeText(this, "Login Successful ", Toast.LENGTH_SHORT).show();
                Intent i = new Intent(MainActivity.this, HomeActivity.class);
                i.putExtra("username", userName);
                startActivity(i);
            } else {
                // display a toast message when user logout of the app.
                ParseUser.logOut();
                Toast.makeText(MainActivity.this, e.getMessage(), Toast.LENGTH_LONG).show();
            }
        });
    }
}
 
