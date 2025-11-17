package com.example.menumanager;

import android.content.SharedPreferences;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;
import org.json.JSONArray;
import org.json.JSONException;
import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    private TextView menuTextView;
    private Button randomButton, addButton, confirmAddButton;
    private EditText newDishInput;
    private LinearLayout addLayout;
    private ListView menuListView;
    
    private List<String> menuList;
    private ArrayAdapter<String> adapter;
    private SharedPreferences preferences;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initViews();
        loadMenuData();
        setupAdapter();
        setupClickListeners();
    }

    private void initViews() {
        menuTextView = findViewById(R.id.menu_text);
        randomButton = findViewById(R.id.random_button);
        addButton = findViewById(R.id.add_button);
        confirmAddButton = findViewById(R.id.confirm_add_button);
        newDishInput = findViewById(R.id.new_dish_input);
        addLayout = findViewById(R.id.add_layout);
        menuListView = findViewById(R.id.menu_list);
        
        preferences = getSharedPreferences("menu_data", MODE_PRIVATE);
        menuList = new ArrayList<>();
    }

    private void loadMenuData() {
        String savedMenu = preferences.getString("menu_list", null);
        if (savedMenu != null) {
            try {
                JSONArray jsonArray = new JSONArray(savedMenu);
                for (int i = 0; i < jsonArray.length(); i++) {
                    menuList.add(jsonArray.getString(i));
                }
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
        
        // 如果没有保存的数据，使用默认菜谱
        if (menuList.isEmpty()) {
            String[] defaultMenu = {
                "盐焗金鲳鱼", "烤牛排", "炒快菜", "凉拌豆腐丝", "火锅", "煮挂面", "蒜蓉粉丝虾",
                "油闷大虾", "黑椒牛柳意面", "葱烧豆腐", "豆角肉焖饭", "奥尔良鸡翅", "冬笋排骨汤",
                "红烧鲫鱼", "柿子椒炒肉", "干锅花菜", "红烧带鱼", "青蒜烧鱼", "土豆鸡翅", "炖猪蹄",
                "牛肉河粉", "干辣椒炒丝瓜", "西红柿鸡蛋面"
            };
            for (String dish : defaultMenu) {
                menuList.add(dish);
            }
            saveMenuData();
        }
    }

    private void setupAdapter() {
        adapter = new ArrayAdapter<String>(this, R.layout.list_item_menu, R.id.dish_name, menuList) {
            @Override
            public View getView(final int position, View convertView, ViewGroup parent) {
                View view = super.getView(position, convertView, parent);
                Button deleteButton = view.findViewById(R.id.delete_button);
                
                deleteButton.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        deleteDish(position);
                    }
                });
                
                return view;
            }
        };
        menuListView.setAdapter(adapter);
    }

    private void setupClickListeners() {
        randomButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (!menuList.isEmpty()) {
                    int randomIndex = (int) (Math.random() * menuList.size());
                    String selectedDish = menuList.get(randomIndex);
                    menuTextView.setText("今天吃: " + selectedDish + " 🍽️");
                } else {
                    menuTextView.setText("菜谱为空，请先添加菜品");
                }
            }
        });

        addButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                addLayout.setVisibility(addLayout.getVisibility() == View.VISIBLE ? View.GONE : View.VISIBLE);
            }
        });

        confirmAddButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String newDish = newDishInput.getText().toString().trim();
                if (!newDish.isEmpty()) {
                    addNewDish(newDish);
                    newDishInput.setText("");
                    addLayout.setVisibility(View.GONE);
                    Toast.makeText(MainActivity.this, "添加成功", Toast.LENGTH_SHORT).show();
                } else {
                    Toast.makeText(MainActivity.this, "请输入菜品名称", Toast.LENGTH_SHORT).show();
                }
            }
        });
    }

    private void addNewDish(String dish) {
        menuList.add(dish);
        adapter.notifyDataSetChanged();
        saveMenuData();
    }

    private void deleteDish(int position) {
        String deletedDish = menuList.get(position);
        menuList.remove(position);
        adapter.notifyDataSetChanged();
        saveMenuData();
        Toast.makeText(this, "已删除: " + deletedDish, Toast.LENGTH_SHORT).show();
    }

    private void saveMenuData() {
        JSONArray jsonArray = new JSONArray();
        for (String dish : menuList) {
            jsonArray.put(dish);
        }
        preferences.edit().putString("menu_list", jsonArray.toString()).apply();
    }
}
