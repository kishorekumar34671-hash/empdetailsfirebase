
# Ex.No:1 To create a employee details fields and to display the employee details using Firebase Database in Android Studio.


## AIM:

To create and display the employee details using Firebase Database in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Artic Fox)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as HelloWorld and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display the employee details in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the DatabaseTable using the firebasedatabase”.
Developed by:KISHORE KUMAR B
Registeration Number :212225240073
AndriodManifest.Xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
EMP.java
```
package com.example.myapplication;

public class Emp {
    public String id, name, age, salary;

    public Emp() {}

    public Emp(String name, String age, String salary) {
        this.name = name;
        this.age = age;
        this.salary = salary;
    }

    public Emp(String id, String name, String age, String salary) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.salary = salary;
    }

}
```
MainActivity.java
```
package com.example.myapplication;

import android.os.Bundle;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

import com.google.firebase.database.*;

import java.util.*;

public class MainActivity extends AppCompatActivity {

    EditText etName, etAge, etSalary;
    Button btnAdd, btnUpdate, btnDelete;
    ListView listView;

    DatabaseReference dbRef;

    ArrayList<String> items = new ArrayList<>();
    ArrayList<String> keys = new ArrayList<>();

    ArrayAdapter<String> adapter;

    String selectedKey = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etName = findViewById(R.id.etName);
        etAge = findViewById(R.id.etAge);
        etSalary = findViewById(R.id.etSalary);

        btnAdd = findViewById(R.id.btnAdd);
        btnUpdate = findViewById(R.id.btnUpdate);
        btnDelete = findViewById(R.id.btnDelete);

        listView = findViewById(R.id.listView);

        dbRef = FirebaseDatabase.getInstance().getReference("Employees");

        adapter = new ArrayAdapter<>(
                this,
                android.R.layout.simple_list_item_1,
                items
        );

        listView.setAdapter(adapter);

        // READ
        dbRef.addValueEventListener(new ValueEventListener() {

            @Override
            public void onDataChange(DataSnapshot snapshot) {

                items.clear();
                keys.clear();

                for (DataSnapshot d : snapshot.getChildren()) {

                    Emp e = d.getValue(Emp.class);

                    if (e != null) {
                        items.add(e.name + " | " + e.age + " | " + e.salary);
                        keys.add(d.getKey());
                    }
                }

                adapter.notifyDataSetChanged();
            }

            @Override
            public void onCancelled(DatabaseError error) {
            }
        });

        // ADD
        btnAdd.setOnClickListener(v -> {

            Emp e = new Emp(
                    etName.getText().toString(),
                    etAge.getText().toString(),
                    etSalary.getText().toString()
            );

            dbRef.push().setValue(e);

            clearFields();
        });

        // UPDATE
        btnUpdate.setOnClickListener(v -> {

            if (selectedKey != null) {

                Emp e = new Emp(
                        etName.getText().toString(),
                        etAge.getText().toString(),
                        etSalary.getText().toString()
                );

                dbRef.child(selectedKey).setValue(e);

                clearFields();

            } else {

                Toast.makeText(
                        this,
                        "Select an item first",
                        Toast.LENGTH_SHORT
                ).show();
            }
        });

        // DELETE
        btnDelete.setOnClickListener(v -> {

            if (selectedKey != null) {

                dbRef.child(selectedKey).removeValue();

                clearFields();

            } else {

                Toast.makeText(
                        this,
                        "Select an item first",
                        Toast.LENGTH_SHORT
                ).show();
            }
        });

        // SELECT ITEM
        listView.setOnItemClickListener((p, v, pos, id) -> {

            selectedKey = keys.get(pos);

            dbRef.child(selectedKey)
                    .get()
                    .addOnSuccessListener(d -> {

                        Emp e = d.getValue(Emp.class);

                        if (e != null) {

                            etName.setText(e.name);
                            etAge.setText(e.age);
                            etSalary.setText(e.salary);
                        }
                    });
        });
    }

    private void clearFields() {

        etName.setText("");
        etAge.setText("");
        etSalary.setText("");

        selectedKey = null;
    }
}
```
*/
```

## OUTPUT
<img width="1920" height="1080" alt="Screenshot 2026-10-29 090250" src="https://github.com/user-attachments/assets/f84eba2e-db39-4dcd-9051-0e60f262741c" />
<img width="1507" height="742" alt="Screenshot 2026-10-29 090331" src="https://github.com/user-attachments/assets/dcf66729-2696-42fd-b5f5-497181bffd30" />







## RESULT
Thus a Simple Android Application create a firebase database and to display the employee details using Firbase Real Time Database in Android Studio is developed and executed successfully.
