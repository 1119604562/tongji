# Java实验
对目录中 Java 源程序文件进行统计，统计内容包括：(1) 目录中每个源程序 文件的总行数和空白行数，文件的字节数；(2) 目录中所有源程序文件合计总行数、合计空 白行数、合计文件的字节数。同时还需要将统计分析的结果保存在根目录的result文件夹中， 方便后续的读取。
# 代码说明
## 1.读入需要统计的目录。

```java
public  static String getPathName()//读入要统计的目录并判断输入目录是否正确
    {
        System.out.println("请输入目录路径");
        Scanner sc=new Scanner(System.in);
        String pathName=sc.nextLine() ;
        while (true)
        {
            root=new File(pathName);
            if(root.isFile())//是否目录
            {
                System.out.println("输入不是目录，请重新输入");
            }else if(!root.exists())//目录是否存在
            {
                System.out.println("目录不存在，请重新输入");
            }
            else break;//目录正确，退出循环

        }

        fileList=new ArrayList<>();
        return pathName;
    }
```
## 2.查找目录下所有.java文件并记入fileList里
```Java
public static void searchFile()//找.java后缀文件
    {
        File[] files=root.listFiles();//获取root下的文件列表

        for (File file : files) 
        {
            if (file.isDirectory())//如果某文件是目录的话
            {
                root = file;
                searchFile();
            } else {
                if (file.getName().endsWith(".java"))
                    fileList.add(file);//是java后缀加到集合里
            }
        }
    }
```
 ## 3.计数并输出结果到result.txt
```Java
 public static void countcount(String pathName)//数数
    {
        double rows=0;//总行数
        double row;//单个文件的行数
        double white;//单个空白行数
        double whites=0;//总的空白行数
        long size; //单个字节数
        long sizes=0;//总的字节数
        String name;
        File result=new File(pathName+"\\result");
        if (!result.exists())result.mkdir();
        BufferedWriter out = null;
        try {
            out = new BufferedWriter(new FileWriter(result+"\\result.txt"));
        } catch (IOException e) {
            e.printStackTrace();
        }
        for (File file:fileList)
        {
            try
         {
             name=file.getName();
             row=countRows(file);
             white=countWhite(file);
             size=countByte(file);
             out.write(name+": 总行数："+row+" , 空白行数："+white+" , 字节数："+size);
             out.newLine();
             out.flush();
             rows+=row;
             sizes+=size;
             whites+=white;
         }catch (IOException e)
         {
             e.printStackTrace();
         }

        }
        try {
            out.write("所有.java文件总行数："+rows+" , 空白行数："+whites+" , 字节数："+sizes);
            out.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```
