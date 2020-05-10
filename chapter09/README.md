# 웹 브라우저 만들기

## 권한 부여하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.webviewexam">

    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```


## layout 정의하기

> activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <EditText
            android:id="@+id/address_edit"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:hint="http://"
            android:imeOptions="actionSearch"
            android:inputType="textUri"
            android:maxLines="1" />

        <Button
            android:id="@+id/move_button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="onClick"
            android:visibility="invisible"
            android:text="이동" />

    </LinearLayout>

    <WebView
        android:id="@+id/web_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

## 메뉴 정의하기

> menu/menu_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:id="@+id/action_back"
        android:title="뒤로 가기"
        android:icon="@drawable/ic_arrow_back"
        app:showAsAction="ifRoom"/>
    <item
        android:id="@+id/action_forword"
        android:title="앞으로 가기"
        android:icon="@drawable/ic_arrow_forward"
        app:showAsAction="ifRoom"/>
    <item
        android:id="@+id/action_refresh"
        android:title="새로 고침"
        android:icon="@drawable/ic_refresh"
        app:showAsAction="ifRoom"/>
</menu>
```

## 자바 소스 코드

> MainActivity.java

```java
public class MainActivity extends AppCompatActivity {

    private EditText mAddressEdit;
    private WebView myWebView;
    private Button mMoveButton;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        myWebView = (WebView) findViewById(R.id.web_view);

        // WebView의 설정
        WebSettings webSettings = myWebView.getSettings();
        webSettings.setJavaScriptEnabled(true);
        myWebView.setWebViewClient(new WebViewClient());

        mAddressEdit = (EditText) findViewById(R.id.address_edit);
        mMoveButton = (Button) findViewById(R.id.move_button);

        mAddressEdit.setOnEditorActionListener(new TextView.OnEditorActionListener() {
            @Override
            public boolean onEditorAction(TextView v, int actionId, KeyEvent event) {
                if (actionId == EditorInfo.IME_ACTION_SEARCH) {
                    mMoveButton.callOnClick();
                    return true;
                }
                return false;
            }
        });

        // 소프트키보드의 돋보기를 클릭했을 때 이동 버튼을 클릭하도록 함
        mAddressEdit.setOnEditorActionListener(new TextView.OnEditorActionListener() {
            @Override
            public boolean onEditorAction(TextView v, int actionId, KeyEvent event) {
                if (actionId == EditorInfo.IME_ACTION_SEARCH) {
                    mMoveButton.callOnClick();

                    // 키보드 내리기
                    InputMethodManager imm = (InputMethodManager)
                            getSystemService(Context.INPUT_METHOD_SERVICE);
                    imm.hideSoftInputFromWindow(v.getWindowToken(), 0);
                    return true;
                }
                return false;
            }
        });


    }

    public void onClick(View view) {
        String address = mAddressEdit.getText().toString();
        if (address.startsWith("http://") == false) {
            address = "http://" + address;
        }
        myWebView.loadUrl(address);

    }

    @Override
    public void onBackPressed() {
        if (myWebView.canGoBack()) {
            myWebView.goBack();
        } else {
            super.onBackPressed();
        }
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        MenuInflater inflater = getMenuInflater();
        inflater.inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            case R.id.action_back:
                // 뒤로 가기
                if (myWebView.canGoBack()) {
                    myWebView.goBack();
                }
                break;
            case R.id.action_forward:
                // 앞으로 가기
                if (myWebView.canGoForward()) {
                    myWebView.goForward();
                }
                break;
            case R.id.action_refresh:
                // 새로고침
                myWebView.reload();
                break;
        }
        return super.onOptionsItemSelected(item);
    }

}
```