# News-Authentication-App

Code for "AndroidManifest.xml" :

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.internshala.app">

    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_app"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".No_Reg"/>
        <activity android:name=".Home_Page" />
        <activity android:name=".Login" />
        <activity android:name=".SignUp" />
        <activity android:name=".MainActivity" />
    </application>

</manifest>

Code for "DatabaseHelper.kt" :

package com.internshala.app

import android.content.ContentValues
import android.content.Context
import android.database.sqlite.SQLiteDatabase
import android.database.sqlite.SQLiteOpenHelper
import android.provider.ContactsContract

class DatabaseHelper(context: Context):SQLiteOpenHelper(context, dbname, factory, version){
    override fun onCreate(db: SQLiteDatabase?) {
        //p0?.execSQL(sql:"create table user(id integer primary key autoincrement,"+"name varchar(30),email varchar(100),password varchar(20),retypepwd varchar(20)"))
    }

    override fun onUpgrade(db: SQLiteDatabase?, oldVersion: Int, newVersion: Int) {
        TODO("Not yet implemented")
    }

    fun insertUserData(name:String,email:String,password:String){
        val db: SQLiteDatabase = writableDatabase
        val values: ContentValues = ContentValues()
        values.put("name",name)
        values.put("email",email)
        values.put("password",password)


        db.insert("user",null,values)
        db.close()
    }

    fun userPresent(email: String,password: String):Boolean{
        val db= writableDatabase
        val query=  "select * from user where email = $email and password = $password"
        val cursor=db.rawQuery(query,null)
        if (cursor.count<=0){
            cursor.close()
            return false
        }
        cursor.close()
        return true
    }

    companion object{
        internal val dbname = "userDB"
        internal val factory = null
        internal val version = 1
    }

}

Code for "Home_Page.kt" :

package com.internshala.app

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle

class Home_Page : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_home__page)
    }
}

Code for "Login.kt" :

package com.internshala.app

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle

class Login : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)
    }
}

Code for "MainActivity.kt" :

package com.internshala.app

import android.os.Bundle
import android.view.View
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import kotlinx.android.synthetic.main.activity_login.*
import kotlinx.android.synthetic.main.activity_main.*
import kotlinx.android.synthetic.main.activity_sign_up.*
import java.sql.Connection

class MainActivity : AppCompatActivity() {

    lateinit var handler: DatabaseHelper
    lateinit var connection: Connection

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        handler = DatabaseHelper(this)

        showHome()

        register.setOnClickListener {
            showRegistration()
        }
        login.setOnClickListener {
            showLogin()
        }
        no_reg.setOnClickListener {
            showNoRegistration()
        }
        to_reg.setOnClickListener {
            showRegistration()
        }
        sign_up.setOnClickListener {
            handler.insertUserData(full_name.text.toString(), email.text.toString(), password.text.toString())
            showHome()
        }
        login_button.setOnClickListener {
            if (handler.userPresent(username.text.toString(),password_login.text.toString()))
                Toast.makeText(this,"Successfully Logged In!!",Toast.LENGTH_SHORT).show()
            else
                Toast.makeText(this,"Invalid Username or Password!!",Toast.LENGTH_SHORT).show()
        }
    }


    private fun showRegistration(){
        signup_layout.visibility=View.VISIBLE
        login_layout.visibility=View.GONE
        home_page.visibility=View.GONE
        no_reg_layout.visibility=View.GONE
    }
    private fun showLogin(){
        login_layout.visibility=View.VISIBLE
        signup_layout.visibility=View.GONE
        home_page.visibility=View.GONE
        no_reg_layout.visibility=View.GONE
        to_reg.visibility=View.GONE
    }
    private fun showNoRegistration(){
        no_reg_layout.visibility=View.VISIBLE
        login_layout.visibility=View.GONE
        signup_layout.visibility=View.GONE
        home_page.visibility=View.GONE
    }
    private fun showHome(){
        home_page.visibility=View.VISIBLE
        login_layout.visibility=View.GONE
        signup_layout.visibility=View.GONE
        no_reg_layout.visibility=View.GONE
    }
    private fun showToReg(){
        to_reg.visibility=View.VISIBLE
        login_layout.visibility=View.GONE
    }

}

Code for "No_Reg.kt" :

package com.internshala.app

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle

class No_Reg : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_no__reg)
    }
}

Code for "SignUp.kt" :

package com.internshala.app

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle

class SignUp : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_sign_up)
    }
}

Code for "activity_home_page.xml" :

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.internshala.app.Home_Page"
    android:background="@drawable/home_page">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="80dp"
        android:layout_marginTop="460dp" />

</RelativeLayout>

Code for "activity_login.xml" :

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.internshala.app.Login"
    android:orientation="vertical"
    android:gravity="center_horizontal"
    android:background="@drawable/bg">

    <EditText
        android:id="@+id/username"
        android:layout_width="match_parent"
        android:layout_height="40dp"
        android:layout_marginTop="160dp"
        android:hint="@string/username" />

    <EditText
        android:id="@+id/password_login"
        android:layout_width="match_parent"
        android:layout_height="40dp"
        android:layout_marginTop="20dp"
        android:hint="@string/password_lgn"/>

    <Button
        android:id="@+id/login_button"
        android:layout_width="match_parent"
        android:layout_height="40dp"
        android:layout_marginTop="20dp"
        android:text="@string/login"/>

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="35dp"
            android:text="@string/oops_you_are_not_registered"/>

        <Button
            android:id="@+id/to_reg"
            android:layout_width="wrap_content"
            android:layout_height="35dp"
            android:paddingStart="10dp"
            android:textStyle="bold"
            android:text="@string/wish_to_register" />

    </LinearLayout>


</LinearLayout>

Code for "activity_main.xml" :

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/home_page"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/home_page"
    tools:context="com.internshala.app.MainActivity"
    android:orientation="vertical"
    android:gravity="center">

    <ImageView
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_marginStart="10dp"
        android:layout_marginTop="150dp"
        android:src="@drawable/app_icon" />

    <Button
        android:id="@+id/login"
        android:layout_width="wrap_content"
        android:layout_height="40dp"
        android:layout_marginTop="50dp"
        android:text="@string/login_btn"
        android:textSize="18sp"
        android:textStyle="bold" />

    <Button
        android:id="@+id/register"
        android:layout_width="wrap_content"
        android:layout_height="40dp"
        android:text="@string/register_btn"
        android:textSize="18sp"
        android:textStyle="bold" />

    <Button
        android:id="@+id/no_reg"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/continue_without_registration"
        android:textSize="18sp"
        android:textStyle="bold" />

    <LinearLayout
        android:layout_width="0dp"
        android:layout_height="0dp" >

        <include
            android:id="@+id/login_layout"
            layout="@layout/activity_login"
            android:visibility="gone"/>
        <include
            android:id="@+id/signup_layout"
            layout="@layout/activity_sign_up"
            android:visibility="gone"/>
        <include
            android:id="@+id/no_reg_layout"
            layout="@layout/activity_no__reg"
            android:visibility="gone"/>

    </LinearLayout>

</LinearLayout>

Code for "activity_no_reg.xml" :

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:tools="http://schemas.android.com/tools"
    android:background="@drawable/bg"
    tools:context="com.internshala.app.No_Reg">

</LinearLayout>

Code for "activity_signup.xml" :

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.internshala.app.SignUp"
    android:orientation="vertical"
    android:gravity="center_horizontal"
    android:background="@drawable/bg">

    <EditText
        android:id="@+id/email"
        android:layout_width="match_parent"
        android:layout_height="40dp"
        android:layout_marginTop="40dp"
        android:hint="@string/email"
        android:textSize="18sp"
        android:textStyle="bold" />

    <EditText
        android:id="@+id/full_name"
        android:layout_width="match_parent"
        android:layout_height="40dp"
        android:layout_marginTop="20dp"
        android:hint="@string/full_name"
        android:inputType="text"
        android:textSize="18sp"
        android:textStyle="bold" />

    <EditText
        android:id="@+id/password"
        android:layout_width="match_parent"
        android:layout_height="40dp"
        android:layout_marginTop="20dp"
        android:hint="@string/password_signup"
        android:inputType="textPassword"
        android:textSize="18sp"
        android:textStyle="bold"/>

    <EditText
        android:id="@+id/retype_password"
        android:layout_width="match_parent"
        android:layout_height="40dp"
        android:layout_marginTop="20dp"
        android:hint="@string/retype_password"
        android:textSize="18sp"
        android:textStyle="bold"/>

    <Button
        android:id="@+id/sign_up"
        android:layout_width="match_parent"
        android:layout_height="40dp"
        android:layout_marginTop="20dp"
        android:text="@string/sign_up_btn" />


</LinearLayout>
