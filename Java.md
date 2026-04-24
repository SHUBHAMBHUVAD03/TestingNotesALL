### java
## Max Number form array
```
public class Test{

public static void main(String[] args) {
int arr[] ={5,6,7,3,9};
int max = arr[0];
for(int i =1; i < arr.length ;i++){
if(max < arr[i]){
max = arr[i];
}
}

System.out.println(max);


                    
}
}
```

## Sum of array
```
public class Test{


public static void main(String[] args) {
int arr[] ={5,6,7,3,9};
int sum = 0;
for(int i =1; i < arr.length ;i++){
   sum += arr[i];
 }

System.out.println(sum);


                    
}
}
```

#### Whet is a differnce between the Array and Arraylist 
    - Array is a fixed size and ArrayList is a Dynamic in a size 
    - Array is used to store a element which are mutable but in array list elements are not mutable
    - Array is a faster than the array list
    - we use array when we need a fixed size variable but in arraylist we use when we need a dynamic oprations

#### divisible by 0 exeption handling
``` 
public static void main(String[] args){
    try{
        int i = 18/0;
    }
    catch(ArithmaticExeption e){
        System.out.println("not divisible by 0 ");
    }
}
```

#### How to connect jdbc 
```
import java.sql.*;

public class JdbcExample {
    public static void main(String[] args) throws Exception {

       Connection con = DriverManager.getConnection("url","User","Pass");
        PreparedStatement ps =
            con.prepareStatement("SELECT * FROM student");

        ResultSet rs = ps.executeQuery();

        while (rs.next()) {
            System.out.println(rs.getInt("id") + " " + rs.getString("name"));
        }

        rs.close();
        ps.close();
        con.close();
    }
}
```