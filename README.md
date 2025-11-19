# Android Login Page – Full A to Z Project (Firebase + API)

A complete Android Login Page app using **Kotlin**, **XML**, **Firebase Authentication**, and **Retrofit API Login**.  
This README contains A-to-Z full setup with all files, dependencies, code, and usage.

---

## 🚀 Features
- Firebase Email/Password Login  
- API Login using Retrofit  
- Simple XML UI  
- Input Validation  
- Clean Kotlin code  

---

## 📁 Project Structure
```
app/
 ├─ java/com.example.loginpage/
 │    ├─ MainActivity.kt
 │    ├─ ApiClient.kt
 │    ├─ ApiService.kt
 │    ├─ LoginResponse.kt
 │    └─ Utils.kt
 └─ res/
      ├─ layout/activity_main.xml
      ├─ values/colors.xml
      ├─ values/strings.xml
      └─ values/themes.xml
```
---

# 🔥 Firebase Setup (A to Z)

## Add Firebase Auth dependency
```gradle
implementation 'com.google.firebase:firebase-auth:23.0.0'
```

---

# 🌐 API Setup (A to Z)

## Add Retrofit Dependencies
```gradle
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3'
```

---

# 🧑‍💻 MainActivity.kt
```kotlin
package com.example.loginpage

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.*
import androidx.lifecycle.lifecycleScope
import com.google.firebase.auth.FirebaseAuth
import kotlinx.coroutines.launch

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val email = findViewById<EditText>(R.id.etEmail)
        val password = findViewById<EditText>(R.id.etPassword)
        val btnFirebase = findViewById<Button>(R.id.btnFirebase)
        val btnApi = findViewById<Button>(R.id.btnApi)

        btnFirebase.setOnClickListener {
            val user = email.text.toString()
            val pass = password.text.toString()

            FirebaseAuth.getInstance().signInWithEmailAndPassword(user, pass)
                .addOnSuccessListener {
                    Toast.makeText(this, "Firebase Login Successful", Toast.LENGTH_SHORT).show()
                }
                .addOnFailureListener {
                    Toast.makeText(this, it.message, Toast.LENGTH_SHORT).show()
                }
        }

        btnApi.setOnClickListener {
            val map = HashMap<String, String>()
            map["email"] = email.text.toString()
            map["password"] = password.text.toString()

            lifecycleScope.launch {
                try {
                    val res = ApiClient.api.login(map)
                    if (res.status) {
                        Toast.makeText(this@MainActivity, "API Login Successful", Toast.LENGTH_SHORT).show()
                    } else {
                        Toast.makeText(this@MainActivity, res.message, Toast.LENGTH_SHORT).show()
                    }
                } catch (e: Exception) {
                    Toast.makeText(this@MainActivity, e.message, Toast.LENGTH_SHORT).show()
                }
            }
        }
    }
}
```

---

# 🌐 ApiService.kt
```kotlin
package com.example.loginpage

import retrofit2.http.Body
import retrofit2.http.POST

interface ApiService {
    @POST("login")
    suspend fun login(@Body data: HashMap<String, String>): LoginResponse
}
```

---

# 🌐 ApiClient.kt
```kotlin
package com.example.loginpage

import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory

object ApiClient {
    private const val BASE_URL = "https://yourapi.com/"

    val api: ApiService by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
            .create(ApiService::class.java)
    }
}
```

---

# 🧾 LoginResponse.kt
```kotlin
package com.example.loginpage

data class LoginResponse(
    val status: Boolean,
    val message: String,
    val token: String?
)
```

---

# 🎨 XML Layout

## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="24dp">

    <EditText
        android:id="@+id/etEmail"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Email" />

    <EditText
        android:id="@+id/etPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Password"
        android:inputType="textPassword"
        android:layout_marginTop="12dp" />

    <Button
        android:id="@+id/btnFirebase"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Login with Firebase"
        android:layout_marginTop="20dp" />

    <Button
        android:id="@+id/btnApi"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Login with API"
        android:layout_marginTop="10dp" />

</LinearLayout>
```

---

## ▶️ How to Run
1. Add `google-services.json`  
2. Update API base URL  
3. Sync Gradle  
4. Run App  

---

## 📜 License
Free to use for learning and development.
