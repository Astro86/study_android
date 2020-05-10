# 액티비티끼리 데이터 주고 받기

## ActivityExam

> activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <EditText
        android:id="@+id/name_edit"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="이름" />

    <EditText
        android:id="@+id/age_edit"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="나이" />

    <Button
        android:id="@+id/submit_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="전송" />
</LinearLayout>
```

> activity_second.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/message_edit_text"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#ffff00" />

    <Button
        android:id="@+id/result_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="결과 전달" />
</LinearLayout>
```

> MainActivity.java

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    public static final int REQUEST_CODE = 1000;
    private EditText mNameEditText;
    private EditText mAgeEditText;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // 화면에 layout 표시
        setContentView(R.layout.activity_main);
        // 이름, 나이
        mNameEditText = (EditText) findViewById(R.id.name_edit);
        mAgeEditText = (EditText) findViewById(R.id.age_edit);
        // 버튼 이벤트 처리
        findViewById(R.id.submit_button).setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        // SecondActivity로 전환하겠다는 intent
        Intent intent = new Intent(this, SecondActivity.class);
        // 이름, 나이 가져와서 intent에 추가
        intent.putExtra("name", mNameEditText.getText().toString());
        intent.putExtra("age", mAgeEditText.getText().toString());
        // intent의 정보를 토대로 다른 Activity를 시작
        startActivityForResult(intent, REQUEST_CODE);
    }

    // SecondActivity에서 돌려받은 결과를 처리하는 콜백
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == REQUEST_CODE
                && resultCode == RESULT_OK
                && data != null) {
            // 결과를 받음
            String result = data.getStringExtra("result");
            // 토스트 메시지 표시
            Toast.makeText(MainActivity.this, result, Toast.LENGTH_SHORT).show();
        }
    }

}
```

> SecondActivity.java

```java
public class SecondActivity extends AppCompatActivity implements View.OnClickListener {
    private TextView mMessageTextView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);
        // 넘어온 값을 화면에 표시
        Intent intent = getIntent();
        String name = intent.getStringExtra("name");
        String age = intent.getStringExtra("age");

        mMessageTextView = (TextView) findViewById(R.id.message_edit_text);
        mMessageTextView.setText(age + "살 " + name);
        // 버튼 이벤트 연결
        findViewById(R.id.result_button).setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        Intent intent = new Intent();
        intent.putExtra("result", mMessageTextView.getText().toString());
        // 결과 전달
        setResult(RESULT_OK, intent);
        // 이 액티비티 종료
        finish();
    }

}
```