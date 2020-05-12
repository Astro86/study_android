# 10장 화면 제약을 극복하는 방법

## 스크롤 뷰 사용하기

> activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.scrollexam.MainActivity">

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <!-- 스크롤뷰는 반드시 하나의 자식만 가질 수 있음 -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Hello World!"
                android:textSize="100dp" />
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Hello World!"
                android:textSize="100dp" />
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Hello World!"
                android:textSize="100dp" />
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Hello World!"
                android:textSize="100dp" />

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Hello World!"
                android:textSize="100dp" />
        </LinearLayout>
    </ScrollView>
</RelativeLayout>
```

> MainActivity.java

```java
package com.example.scrollexam;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

# 스크롤 뷰 사용하기

## ScrollExam

> activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.adapterviewexam.MainActivity">

    <ListView
        android:id="@+id/list_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:listitem="@layout/item_weather" />
</RelativeLayout>
```

> item_weather.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal">

    <ImageView
        android:id="@+id/weather_image"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:src="@mipmap/ic_launcher" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:orientation="vertical"
        android:padding="8dp">

        <TextView
            android:id="@+id/city_text"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="도시명"
            android:textSize="30sp" />

        <TextView
            android:id="@+id/temp_text"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:gravity="end"
            android:text="기온" />

    </LinearLayout>

</LinearLayout>
```

> MainActivity.java

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 자료
//        ArrayList<String> data = new ArrayList<>();
//        for (int i = 0; i < 30; i++) {
//            data.add("data " + i);
//        }

        ArrayList<Weather> data = new ArrayList<>();
        data.add(new Weather("수원", "25도", "맑음"));
        data.add(new Weather("서울", "26도", "비"));
        data.add(new Weather("안양", "24도", "구름"));
        data.add(new Weather("부산", "29도", "구름"));
        data.add(new Weather("인천", "23도", "맑음"));
        data.add(new Weather("대구", "28도", "비"));
        data.add(new Weather("용인", "25도", "비"));


        // 어댑터
//        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
//                android.R.layout.simple_list_item_1, data);

        MyFirstAdapter adapter = new MyFirstAdapter(data);

        // 뷰
        ListView listView = (ListView) findViewById(R.id.list_view);
        listView.setAdapter(adapter);


    }
}
```

> MyFirstAdapter

```java
public class MyFirstAdapter extends BaseAdapter {
    private final List<Weather> mData;

    private Map<String, Integer> mWeatherImageMap;


    // List 를 구현한 모든 것(ArrayList 등) 을 받는 생성자
    public MyFirstAdapter(List<Weather> data) {
        mData = data;

        mWeatherImageMap = new HashMap<>();
        mWeatherImageMap.put("맑음", R.drawable.sunny);
        mWeatherImageMap.put("폭설", R.drawable.blizzard);
        mWeatherImageMap.put("구름", R.drawable.cloudy);
        mWeatherImageMap.put("비", R.drawable.rainy);
        mWeatherImageMap.put("눈", R.drawable.snow);
    }

    // 아이템의 갯수
    @Override
    public int getCount() {
        return mData.size();
    }

    // position 번째의 아이템
    @Override
    public Object getItem(int position) {
        return mData.get(position);
    }

    // position 번째의 아이디
    @Override
    public long getItemId(int position) {
        return position;
    }

    // position 번째의 아이템의 View 를 구성하는 부분
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        ViewHolder holder;

        if (convertView == null) {
            holder = new ViewHolder();
            convertView = LayoutInflater.from(parent.getContext())
                    .inflate(R.layout.item_weather, parent, false);

            // 날씨, 도시, 기온 View
            ImageView weatherImage = (ImageView) convertView.findViewById(R.id.weather_image);
            TextView cityText = (TextView) convertView.findViewById(R.id.city_text);
            TextView tempText = (TextView) convertView.findViewById(R.id.city_text);

            holder.weatherImage = weatherImage;
            holder.cityText = cityText;
            holder.tempText = tempText;

            convertView.setTag(holder);
        } else {
            holder = (ViewHolder) convertView.getTag();
        }


        // 현재 position 의 날씨 데이터
        Weather weather = mData.get(position);

        // 데이터 설정 => 홀더에 저장
        holder.cityText.setText(weather.getCity());
        holder.tempText.setText(weather.getTemp());
        holder.weatherImage.setImageResource(mWeatherImageMap.get(weather.getWeather()));

        return convertView;
    }

    static class ViewHolder {
        ImageView weatherImage;
        TextView cityText;
        TextView tempText;
    }
}
```

> Weather

```java
public class Weather {
    private String city;        // 도시명
    private String temp;        // 기온
    private String weather;     // 날씨 (맑음, 비, 구름, 눈)

    public Weather(String city, String temp, String weather) {
        this.city = city;
        this.temp = temp;
        this.weather = weather;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    public String getTemp() {
        return temp;
    }

    public void setTemp(String temp) {
        this.temp = temp;
    }

    public String getWeather() {
        return weather;
    }

    public void setWeather(String weather) {
        this.weather = weather;
    }

    @Override
    public String toString() {
        final StringBuffer sb = new StringBuffer("Weather{");
        sb.append("city='").append(city).append('\'');
        sb.append(", temp='").append(temp).append('\'');
        sb.append(", weather='").append(weather).append('\'');
        sb.append('}');
        return sb.toString();
    }
}
```