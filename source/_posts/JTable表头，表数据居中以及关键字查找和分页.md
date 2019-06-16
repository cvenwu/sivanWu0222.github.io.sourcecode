---
title: JTable表头，表数据居中以及关键字查找和分页
date: 2017-05-05 09:19:37
tags: [Java,JTable]
categories:
- Java
- Swing
---

## JTable 设置表头居中
> 通过表格默认的渲染器和引用JLabel的center来使得表头居中

```Java
  ((DefaultTableCellRenderer)table.getTableHeader().getDefaultRenderer()).setHorizontalAlignment(JLabel.CENTER);
```
----------

## JTable 设置表数据居中
> 有时我们需要让表格数据居中显示，而不是默认的左对齐，可以给用户一个更好的体验！


```Java
  DefaultTableCellRenderer renderer=new DefaultTableCellRenderer();
  renderer.setHorizontalAlignment(DefaultTableCellRenderer.CENTER);
  table.setDefaultRenderer(Object.class, renderer);
```
<!--more-->
----------
## JTable 设置表头不可重排序
> 当我们使用JTable控件，发现如果移动一个表头到另一个表头处，也是可以的，如果我们不想要这种效果，便可以执行如下代码

```Java
  table.getTableHeader().setReorderingAllowed(false);
```
----------
## JTable 设置表格内容不可编辑
> 我们使用JTable,有时只是为了展示一个关系给用户，并不希望用户可以编辑，但是我们使用JTable,只要点击表格一处内容就可以让用户编辑，因此可以执行如下代码



在我们新建表格的时候实现 isCellEditable方法便可以
```Java
  private JTable table  = new JTable(){
  		public boolean isCellEditable(int row, int column) {
  			return false;
  		};
  	};
```


----------
## JTable 设置表格行高

```Java
  table.setRowHeight(30);   //设置表格行高为30px
```
----------
## JTable 实现表格的排序以及关键字查找
> 对于表格，经常需要我们去进行排序和关键字的查找


### 排序
- 设置表格的表模型，并设置表格的setRowSorter(JDK1.6出现，用于对JTable实现排序和过滤，此方法清除该选择并重置所有可变行高度)
- 使用setRowSorter实现的是表格粗粒度排序，也就是通过将每行每一列内容转换成字符串进行排序，

```Java
  table.setRowSorter(sorter);
```



### 过滤（关键字查找）
> 对于数据量大的表格我们经常通过关键字来进行查找，这里使用了Java中的RowFilter用于从表格模型中过滤条目，对于不满足关键字的将不会显示在表格当中
> RowFilter 与 JTable 相关联，一个条目对应于JTable的一行记录

- 首先要产生一个文本框（输入要查询的内容，）
- 设置表格的模型以及setRowSorter()
- 设置sorter的setRowFilter,用于实现文本过滤

```Java
import java.awt.BorderLayout;
import java.awt.Dimension;
import java.awt.EventQueue;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.JTextField;
import javax.swing.RowFilter;
import javax.swing.UIManager;
import javax.swing.border.EmptyBorder;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.JTableHeader;
import javax.swing.table.TableModel;
import javax.swing.table.TableRowSorter;

public class SearchTable extends JFrame {

  /**
   *
   */
  private static final long serialVersionUID = -3619887890741475524L;
  private JPanel contentPane;
  private JTable table;
  private JTextField textField;
  private TableRowSorter<TableModel> sorter = new TableRowSorter<TableModel>();;

  /**
   * Launch the application.
   */
  public static void main(String[] args) {
      try {
          UIManager.setLookAndFeel("com.sun.java.swing.plaf.nimbus.NimbusLookAndFeel");
      } catch (Throwable e) {
          e.printStackTrace();
      }
      EventQueue.invokeLater(new Runnable() {
          public void run() {
              try {
                  SearchTable frame = new SearchTable();
                  frame.setVisible(true);
              } catch (Exception e) {
                  e.printStackTrace();
              }
          }
      });
  }

  /**
   * Create the frame.
   */
  public SearchTable() {
      addWindowListener(new WindowAdapter() {
          @Override
          public void windowActivated(WindowEvent e) {
              do_this_windowActivated(e);
          }
      });
      setTitle("\u652F\u6301\u67E5\u627E\u529F\u80FD\u7684\u8868\u683C");
      setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
      setBounds(100, 100, 450, 300);
      contentPane = new JPanel();
      contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
      contentPane.setLayout(new BorderLayout(0, 0));
      setContentPane(contentPane);

      JPanel panel = new JPanel();
      contentPane.add(panel, BorderLayout.NORTH);

      JLabel label = new JLabel("\u5173\u952E\u5B57\uFF1A");
      label.setFont(new Font("微软雅黑", Font.PLAIN, 16));
      panel.add(label);

      textField = new JTextField();
      textField.setFont(new Font("微软雅黑", Font.PLAIN, 16));
      panel.add(textField);
      textField.setColumns(20);

      JPanel buttonPanel = new JPanel();
      contentPane.add(buttonPanel, BorderLayout.SOUTH);

      JButton button = new JButton("\u67E5\u627E");
      button.addActionListener(new ActionListener() {
          public void actionPerformed(ActionEvent e) {
            sorter.setRowFilter(RowFilter.regexFilter(textField.getText()));
          }
      });
      button.setFont(new Font("微软雅黑", Font.PLAIN, 16));
      buttonPanel.add(button);

      JScrollPane scrollPane = new JScrollPane();
      contentPane.add(scrollPane, BorderLayout.CENTER);

      table = new JTable();
      table.setFont(new Font("微软雅黑", Font.PLAIN, 14));
      table.setRowHeight(30);
      JTableHeader header = table.getTableHeader();
      header.setFont(new Font("微软雅黑", Font.PLAIN, 16));
      header.setPreferredSize(new Dimension(header.getWidth(), 35));
      scrollPane.setViewportView(table);
  }

  protected void do_this_windowActivated(WindowEvent e) {
      DefaultTableModel tableModel = (DefaultTableModel) table.getModel();
      tableModel.setRowCount(0);
      tableModel.setColumnIdentifiers(new Object[] { "书名", "出版社", "出版时间", "丛书类别", "定价" });
      tableModel.addRow(new Object[] { "Java从入门到精通（第2版）", "清华大学出版社", "2010-07-01", "软件工程师入门丛书", "59.8元" });
      tableModel.addRow(new Object[] { "PHP从入门到精通（第2版）", "清华大学出版社", "2010-07-01", "软件工程师入门丛书", "69.8元" });
      tableModel.addRow(new Object[] { "Visual Basic从入门到精通（第2版）", "清华大学出版社", "2010-07-01", "软件工程师入门丛书", "69.8元" });
      tableModel.addRow(new Object[] { "Visual C++从入门到精通（第2版）", "清华大学出版社", "2010-07-01", "软件工程师入门丛书", "69.8元" });
      sorter.setModel(tableModel);
      table.setRowSorter(sorter);
  }

}

```

----------

## 实现表格的分页
> 对于数据量大的表格，我们不仅仅需要查找和过滤，更需要我们实现表格的分页，对于如何分页，这里实现了表格分页的一个小算法

思路：
- 得到数据总数
- 得到所需页数 = Math.ceil（数据总数 / 每页计划存放的数据数）
- 显示当前页数，编写首页，上一页，当前页数，下一页，最后一页的监听事件
如果用户不输入当期页数，则就显示当前页数，如果输入了一个页数则实现跳转（需要进行判断页数是否合法）
如果用户点击首页，跳转到首页
如果用户点击上一页，则需要判断是否存在上一页，如果存在则跳转
如果用户点击下一页，下一页存在，如果是最后一页显示剩余的数据，如果不是，显示该页的15条记录；下一页不存在，告诉用户已经是最后一页
如果用户点击最后一页，跳转到最后一页

数据需要用户自己进行添加，每页显示15行数据，如果用户需要，可以进行更改

```Java

import java.awt.BorderLayout;
import java.awt.Dimension;
import java.awt.EventQueue;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Vector;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

import javax.swing.JButton;
import javax.swing.JComboBox;
import javax.swing.JDialog;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.JTextField;
import javax.swing.RowFilter;
import javax.swing.UIManager;
import javax.swing.border.EmptyBorder;
import javax.swing.table.DefaultTableCellRenderer;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.JTableHeader;
import javax.swing.table.TableModel;
import javax.swing.table.TableRowSorter;


/**			
   *
   * @author SiVan
   * @time 2017年4月23日 下午7:13:18
   * TODO	实现表格分页
   */
public class QueryPurchase extends JDialog {

private static final long serialVersionUID = -3619887890741475524L;
  private JPanel contentPane;
  /*新建一个表格并设置表格内容不可以编辑*/
  private JTable table  = new JTable(){
  public boolean isCellEditable(int row, int column) {
    return false;
  };
};
  private JTextField textField;
  private TableRowSorter<TableModel> sorter = new TableRowSorter<TableModel>();;

  Vector<String> header ;
private String url = "jdbc:mysql://localhost:3306/erp";
private String user = "root";
private String password = "1018222wxw";
Vector<Vector<String>> dataVector;		/*存放所有数据*/
JTextField text;						/*显示当前页数*/
Vector<Vector<String>> data;			/*存放所要显示的每一页的数据*/
int n = 0;									/*得到数据总数*/
  /**
   * Launch the application.
   */
  public static void main(String[] args) {
      try {
          UIManager.setLookAndFeel("com.sun.java.swing.plaf.nimbus.NimbusLookAndFeel");
      } catch (Throwable e) {
          e.printStackTrace();
      }
      EventQueue.invokeLater(new Runnable() {
          public void run() {
              try {
                QueryPurchase frame = new QueryPurchase();
                  frame.setVisible(true);
              } catch (Exception e) {
                  e.printStackTrace();
              }
          }
      });
  }

  /**
   * Create the frame.
   */
  public QueryPurchase() {

    header = new Vector<String>();
  header.add("进货编号");
  header.add("供应商名称");
  header.add("花盆编号");
  header.add("花盆名称");
  header.add("联系人");
  header.add("数量");
  header.add("单价");
  header.add("总价");
  header.add("进货时间");

  data = new Vector<Vector<String>>();
  dataVector = new Vector<Vector<String>>();
  text = new JTextField();					/*显示当前页数*/

  /*
  对于初次添加的数据，需要判断数据是否填充满第一页，如果填充满则第一页显示15条记录，
  反之显示用户所添加的记录数目
  */
  if(n / 15 >= 1){
    for (int i = 0; i < 15; i++) {
      data.add(dataVector.get(i));
    }
  }else{
    for (int i = 0; i < n % 15; i++) {
      data.add(dataVector.get(i));
    }

  }

  final DefaultTableModel model = new DefaultTableModel(data,header);
  table.setModel(model);
  sorter.setModel(model);
      table.setRowSorter(sorter);


  textField = new JTextField();
      textField.setFont(new Font("微软雅黑", Font.PLAIN, 16));
  textField.addKeyListener(new KeyAdapter() {
    @Override
    public void keyReleased(KeyEvent e) {

      sorter.setRowFilter(RowFilter.regexFilter(textField.getText()));

    }
  });


      this.setSize(1000, 650);
      this.setLocationRelativeTo(null);
      contentPane = new JPanel();
      contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
      contentPane.setLayout(new BorderLayout(0, 0));
      setContentPane(contentPane);

      JPanel panel = new JPanel();
      contentPane.add(panel, BorderLayout.NORTH);

      JLabel label = new JLabel("\u5173\u952E\u5B57\uFF1A");
      label.setFont(new Font("微软雅黑", Font.PLAIN, 16));
      panel.add(label);


      panel.add(textField);
      textField.setColumns(20);

      JPanel buttonPanel = new JPanel();
      contentPane.add(buttonPanel, BorderLayout.SOUTH);

      JButton first = new JButton("首页");
      JButton previous = new JButton("上一页");
      JButton next = new JButton("下一页");
      JButton last = new JButton("尾页");
      JLabel page = new JLabel("当前页数:");

      ((DefaultTableCellRenderer)table.getTableHeader().getDefaultRenderer()).setHorizontalAlignment(JLabel.CENTER);

      first.addActionListener(new ActionListener() {

    @Override
    public void actionPerformed(ActionEvent e) {

      text.setText("1");
      data.removeAllElements();
      if(n / 15 >= 1){
        for (int i = 0; i < 15; i++) {
          data.add(dataVector.get(i));
        }
      }else{
        for (int i = 0; i < n%15; i++) {
          data.add(dataVector.get(i));
        }
      }
          DefaultTableModel model = new DefaultTableModel(data,header);
          table.setModel(model);
    }
  });

      previous.addActionListener(new ActionListener() {

    @Override
    public void actionPerformed(ActionEvent e) {

      String page = text.getText();
      if("1".equals(page)){
        JOptionPane.showMessageDialog(null, "对不起，现在已经是首页了");
        return;
      }
      int k = Integer.parseInt(page);
      text.setText(k - 1 + "");
      data.removeAllElements();
      for (int i = (k - 2) * 15; i < (k - 2) * 5 + 15; i++) {
        data.add(dataVector.get(i));
      }
          DefaultTableModel model = new DefaultTableModel(data,header);
          table.setModel(model);
    }
  });

      next.addActionListener(new ActionListener() {

    @Override
    public void actionPerformed(ActionEvent e) {

      int page = Integer.parseInt(text.getText());
      int k = n % 15;
      if(k == 0){
        if(page == n / 15){
          JOptionPane.showMessageDialog(null, "已经是最后一页了");
          return;
        }
      }else{
        if(page == n / 15 + 1)
        {
          JOptionPane.showMessageDialog(null, "已经是最后一页了");
          return;
        }
        if(!(page == n / 15 )){
          k = 15;
        }
        text.setText(page + 1 + "");
      }
      data.removeAllElements();
      for (int i = page * 15; i < page * 15 +  k; i++) {
        data.add(dataVector.get(i));
      }
      DefaultTableModel model = new DefaultTableModel(data,header);
          table.setModel(model);
    }
  });

      last.addActionListener(new ActionListener() {

    @Override
    public void actionPerformed(ActionEvent e) {

      int k = n % 15;
      data.removeAllElements();
      for(int i = n - k ; i <= n - 1; i++){
        data.add(dataVector.get(i));
      }
      DefaultTableModel model = new DefaultTableModel(data,header);
          table.setModel(model);
    }
  });

      text.addKeyListener(new KeyAdapter() {
        @Override
        public void keyReleased(KeyEvent e) {

          int k = n % 15;
          int page = 0;
          try{
             page = Integer.parseInt(text.getText());
          }catch(Exception e1){
            JOptionPane.showMessageDialog(null, "请注意页数格式");
            e1.printStackTrace();
            return;
          }
          if(page < 1){
            JOptionPane.showMessageDialog(null, "页数非法");
            return;
          }
          if(k == 0){
            if( page > n/15){
              JOptionPane.showMessageDialog(null, "页数非法");
              return;
            }
            k = 15;
          }else{
            if( page > n/15 + 1){
              JOptionPane.showMessageDialog(null, "页数非法");
              return;
            }
            if( page < n/15 + 1){
              k = 15;
            }

          }
      data.removeAllElements();
      for(int i = (page - 1) * 15 ; i < (page - 1) * 15 + k; i++){
        data.add(dataVector.get(i));
      }
      DefaultTableModel model = new DefaultTableModel(data,header);
          table.setModel(model);
        }
  });
      first.setFont(new Font("微软雅黑", Font.PLAIN, 16));
      previous.setFont(new Font("微软雅黑", Font.PLAIN, 16));
      next.setFont(new Font("微软雅黑", Font.PLAIN, 16));
      last.setFont(new Font("微软雅黑", Font.PLAIN, 16));
      page.setFont(new Font("微软雅黑", Font.PLAIN, 16));
      text.setColumns(5);



      buttonPanel.add(first);
      buttonPanel.add(previous);
      buttonPanel.add(page);
      buttonPanel.add(text);
      buttonPanel.add(next);
      buttonPanel.add(last);
      JScrollPane scrollPane = new JScrollPane();
      contentPane.add(scrollPane, BorderLayout.CENTER);

      /*设置表头不可重排序*/
      table.getTableHeader().setReorderingAllowed(false);
      /*设置表格字体*/
      table.setFont(new Font("微软雅黑", Font.PLAIN, 14));
      /*设置表格行高*/
      table.setRowHeight(30);
      /*设置表数据居中*/
      DefaultTableCellRenderer renderer=new DefaultTableCellRenderer();
      renderer.setHorizontalAlignment(DefaultTableCellRenderer.CENTER);
      table.setDefaultRenderer(Object.class, renderer);
      /*设置表头的字体以及宽度高度*/
      JTableHeader header1 = table.getTableHeader();
      header1.setFont(new Font("微软雅黑", Font.PLAIN, 16));
      header1.setPreferredSize(new Dimension(header1.getWidth(), 35));
      scrollPane.setViewportView(table);
      this.setVisible(true);
  }

}

```


----------
