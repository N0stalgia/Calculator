package calculator;
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.*;
/*
思路：最外面用background包裹，并设置边距。
		background里面NORTH位置用一个JTextField. CENTER是一个3行1列grid的mainPanel,每一行一个panel(分别用grid初始化)
		background加三个panel一个textfield;
*/
public class Calculator {
	//创建一个String数组，里面的内容为按钮显示的内容
	//创建一个ArrayList，类型为JButton,用循环初始化这些按钮，button的参数用String数组中的元素。
	String [] str = {"7","8","9","*",
					 "4","5","6","-",
					 "1","2","3","+"};
	ArrayList<JButton> topButList ;
	ArrayList<JButton> midButList ;
	ArrayList<JButton> botButList ;
	
	JTextField field;
	
	double doubleSum = 0.0;
	int intSum = 0;
	
	
	String otherCharacter = "";
	String firstNum = "";				//第一个数字可以是负数，考虑进去
	String secondNum = "";			//只能是正数
	String firstOperator = "";		//
	String secondOperator = "";		//如果是等号，直接输出结果，如果是其他的符号，将结果转入firstNum，操作符转入firstOperator
	
	boolean isOthChars = false;
	boolean isFstDot = false;		//输入的数可能有两种，一种是整数，一种是小数，输入的是整数，要用String转整数，小数，用String转小数
	boolean isSecDot = false;		//可能两个数都是小数
	boolean isFirstOperator = false;		//是第一个操作符，继续进行运算，将SecNun设置为true,FstNum设置为false
	boolean isSecondOperator = false;		//第二个操作符，将两个数进行运算。
	boolean isFstNum = false;				//为true，将数字输入firstNum中
	boolean isSecNum = false;				//为true，将数字输入secondNum中
	
	public static void main (String [] args) {
		System.out.println("isFstNum\tisFirstOperator\tisSecNum\tisOthChars");
		new Calculator().setupGui();
	}
	
	
	/*
	用一个panel，设置Layout为borderLayout，
	一个panel2在左边，占据CENTER，panel3按照grid(1,2)排版在右边，占据EAST？
	*/
	
	public void setupGui () {
		JFrame frame = new JFrame("Calculator");		//图形化界面的名称
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		
		BorderLayout layout = new BorderLayout();		//设置背景，类似frame的排版，用于替代frame
		JPanel background = new JPanel(layout);			//设置panel的排版模式，为BorderLayout
		background.setBorder(BorderFactory.createEmptyBorder(10,10,10,10));		//设置边距
		
		GridLayout grid = new GridLayout(6,1);		//一个6行一列的网格，用于排版计算器的按钮
		setGridGap(grid);
		JPanel mainPanel = new JPanel(grid);
		
		//一个textfield，当作计算器的显示屏
		field= new JTextField(16);
		field.setEditable(false);
		// field.requestFocus();
		mainPanel.add(field);
		
		//最上面的三个按钮
		topButList = new ArrayList<JButton> ();
		JPanel top = new JPanel();		//用大的panel把三个按钮包裹起来，因为mainPanel每行只能加一列元素
		
		topButList.add( new JButton("AC") );	//清零
		topButList.add( new JButton("<") );		//回退
		topButList.add( new JButton("÷") );		//除
		
		topButList.get(0).setPreferredSize(new Dimension(105, 30)); //设置按钮更喜欢的大小,但是这个和grid的排版冲突，所以加到panel里面了
		topButList.get(1).setPreferredSize(new Dimension(50, 30)); 
		topButList.get(2).setPreferredSize(new Dimension(50, 30)); 
		
		topButList.get(0).addActionListener(new TopBut0Listener());
		topButList.get(1).addActionListener(new TopBut1Listener());
		topButList.get(2).addActionListener(new TopBut2Listener());
		
		top.add( topButList.get(0) );
		top.add( topButList.get(1) );
		top.add( topButList.get(2) );
		
		mainPanel.add(top);
		
		//中间有三行按钮，大小都是相同的，所以用循环初始化，在最上面创建的String数组派上用场了
		JPanel mid1 = new JPanel();
		JPanel mid2 = new JPanel();
		JPanel mid3 = new JPanel();
		
		midButList = new ArrayList<JButton> ();
		//为12个按钮进行初始化
		for (int i=0; i<12; i++) {			
			JButton c = new JButton(str[i]);
			midButList.add(c);
			c.setPreferredSize(new Dimension(50, 30));
		}
		midButList.get(0).addActionListener(new midBut0Listener() );
		midButList.get(1).addActionListener(new midBut1Listener() );
		midButList.get(2).addActionListener(new midBut2Listener() );
		midButList.get(3).addActionListener(new midBut3Listener() );
		
		midButList.get(4).addActionListener(new midBut4Listener() );
		midButList.get(5).addActionListener(new midBut5Listener() );
		midButList.get(6).addActionListener(new midBut6Listener() );
		midButList.get(7).addActionListener(new midBut7Listener() );
		
		midButList.get(8).addActionListener(new midBut8Listener() );
		midButList.get(9).addActionListener(new midBut9Listener() );
		midButList.get(10).addActionListener(new midBut10Listener() );
		midButList.get(11).addActionListener(new midBut11Listener() );
		
		
		
		
		
		//把按钮加到中间的三个panel上
		addMidButton(mid1,mid2,mid3);
		
		mainPanel.add(mid1);
		mainPanel.add(mid2);
		mainPanel.add(mid3);
		
		//底部的按钮
		botButList = new ArrayList<JButton> ();
		JPanel bottom = new JPanel();
		
		botButList.add( new JButton("0") );
		botButList.add( new JButton(".") );
		botButList.add( new JButton("=") );
		
		botButList.get(0).setPreferredSize(new Dimension(105, 30)); //设置按钮更喜欢的大小,这个和grid的排版冲突
		botButList.get(1).setPreferredSize(new Dimension(50, 30)); 
		botButList.get(2).setPreferredSize(new Dimension(50, 30)); 
		
		bottom.add( botButList.get(0) );
		bottom.add( botButList.get(1) );
		bottom.add( botButList.get(2) );
		
		botButList.get(0).addActionListener(new botBut0Listener() );
		botButList.get(1).addActionListener(new botBut1Listener() );
		botButList.get(2).addActionListener(new botBut2Listener() );
		
		mainPanel.add(bottom);
		
		
		
		background.add(BorderLayout.CENTER,mainPanel);

		frame.getContentPane().add(BorderLayout.CENTER,background);
		frame.setBounds(30,30,300,300);
		frame.setVisible(true);
		
	}
	//设置排版的垂直和水平间距
	public void setGridGap(GridLayout g) {
		g.setVgap(3);
		g.setHgap(3);
		
	}
	//把按钮加到中间的三个panel上
	public void addMidButton (JPanel mid1,JPanel mid2,JPanel mid3) {
		int i = 0;
		for (i=i; i<4; i++) {
			mid1.add(midButList.get(i));
		}
		for (i=i; i<8; i++) {
			mid2.add(midButList.get(i));
		}
		for (i=i; i<12; i++) {
			mid3.add(midButList.get(i));
		}
	}
	
	//计算两个数的结果
	public void calculate() {
		if (isFstDot==true) {		//第一个数是小数，第二个数是小数
			doubleSum = Double.parseDouble(firstNum);
			
			if (isSecDot==true) {
				if (firstOperator.equals("+") ) {
					doubleSum = doubleSum + Double.parseDouble(secondNum);
				} else if (firstOperator.equals("-") ) {
					doubleSum = doubleSum - Double.parseDouble(secondNum);
				} else if (firstOperator.equals("*") ) {
					doubleSum = doubleSum * Double.parseDouble(secondNum);
				} else if (firstOperator.equals("÷") ) {
					doubleSum = doubleSum / Double.parseDouble(secondNum);
				}
			}
			else {			//第一个数是小数，第二个数是整数
				if (firstOperator.equals("+") ) {
					doubleSum = doubleSum + Integer.parseInt(secondNum);
				} else if (firstOperator.equals("-") ) {
					doubleSum = doubleSum - Integer.parseInt(secondNum);
				} else if (firstOperator.equals("*") ) {
					doubleSum = doubleSum * Integer.parseInt(secondNum);
				} else if (firstOperator.equals("÷") ) {
					doubleSum = doubleSum / Integer.parseInt(secondNum);
				}
			}
			
			setNextOperation(doubleSum);
		}
		
		else {			//第一个数是整数，第二个数是小数
			intSum = Integer.parseInt(firstNum);
			
			if (isSecDot==true) {
				if (firstOperator.equals("+") ) {
					doubleSum = intSum + Double.parseDouble(secondNum);
				} else if (firstOperator.equals("-") ) {
					doubleSum = intSum - Double.parseDouble(secondNum);
				} else if (firstOperator.equals("*") ) {
					doubleSum = intSum * Double.parseDouble(secondNum);
				} else if (firstOperator.equals("÷") ) {
					doubleSum = intSum / Double.parseDouble(secondNum);
				}
				
				setNextOperation(doubleSum);
			}
			else {			//第一个数是整数，第二个数是整数
				if (firstOperator.equals("+") ) {
					intSum = intSum + Integer.parseInt(secondNum);
				} else if (firstOperator.equals("-") ) {
					intSum = intSum - Integer.parseInt(secondNum);
				} else if (firstOperator.equals("*") ) {
					intSum = intSum * Integer.parseInt(secondNum);
				} else if (firstOperator.equals("÷") ) {
					intSum = intSum / Integer.parseInt(secondNum);
				}
				
				setNextOperation(intSum);
			}
		}
	}
	void setNextOperation (double results) {
		
		firstNum = String.format("%.3f",results)+"";
		isFstNum = true;
		isFstDot = true;
		// System.out.println(isSecondOperator);
		if (secondOperator.equals("=") ) {
			field.setText(firstNum);
			firstOperator = "";	
			isFirstOperator = false;
			
		}
		else {
			firstOperator = secondOperator;
			isFirstOperator = true;
			field.setText(firstNum+firstOperator);
			
		}
		otherCharacter = "";			
		secondNum = "";			
		secondOperator = "";
		
		doubleSum = 0.0;
		intSum = 0;
		
		isOthChars = false;
		isSecDot = false;
		isSecNum = false;
		isSecondOperator = false;
	}
	void setNextOperation (int results) {
		
		firstNum = ""+results;
		isFstNum = true;
		// System.out.println(isSecondOperator);
		if (secondOperator.equals("=") ) {
			field.setText(firstNum);
			firstOperator = "";	
			isFirstOperator = false;
			
		}
		else {
			firstOperator = secondOperator;
			isFirstOperator = true;
			field.setText(firstNum+firstOperator);
			
		}
		otherCharacter = "";			
		secondNum = "";			
		secondOperator = "";
		
		doubleSum = 0.0;
		intSum = 0;
		
		isOthChars = false;
		isFstDot = false;
		isSecDot = false;
		isSecNum = false;
		isSecondOperator = false;
	}
	//创建监听动作
	class TopBut0Listener implements ActionListener {		//清零按钮
		public void actionPerformed (ActionEvent event) {
			otherCharacter = "";
			firstNum = "";				
			secondNum = "";			
			firstOperator = "";	
			secondOperator = "";
			
			doubleSum = 0.0;
			intSum = 0;
			
			isOthChars = false;
			isFstDot = false;
			isSecDot = false;
			isFstNum = false;
			isSecNum = false;
			isFirstOperator = false;
			isSecondOperator = false;
			
			field.setText("");
			// field.requestFocus();
		}
	}
	//删除将其分为三个部分，第二个操作符是不允许删除的，一旦按下第二个操作符就会进行运算
	//从secondNum开始删除，删完了就设置成false。再删firstOperator，再删firstNum。
	class TopBut1Listener implements ActionListener {		//删除按钮
		public void actionPerformed (ActionEvent event) {
			if (isOthChars==true) {		//
				otherCharacter = otherCharacter.substring(0,otherCharacter.length() -1);
				field.setText(otherCharacter);
				//如果firstNum和firstOperator都是true，保持这两个状态不变，判断othercharacter的长度，若相等，说明othercharacter删完了
				if (otherCharacter.length() ==(firstNum.length() +firstOperator.length() )) {
					isOthChars = false;
				}
			}
			
			else if (isSecNum==true) {
				
				if (secondNum.length() >0) {		//secondNum的长度一直在变化，所以，每发生一次长度减1就OK
					secondNum = secondNum.substring(0,secondNum.length() -1);
					field.setText(firstNum+firstOperator+secondNum);
				}
				if (secondNum.length()==0) {				//长度为0时，说明secondNum被删完了
					isSecNum = false;
				}
			}
			else if (isFirstOperator==true) {		//就一个字符，删一下，什么都没了，直接状态还原就OK
				firstOperator = "";
				isFirstOperator = false;
				field.setText(firstNum);
			}
			else if (isFstNum==true) {			//
				if (firstNum.length() >0) {
					firstNum = firstNum.substring(0,firstNum.length() -1);
					field.setText(firstNum);
				}
				if (firstNum.length()==0) {				
					isFstNum = false;
				}
			}
			System.out.println(isFstNum+"\t"+isFirstOperator+"\t"+isSecNum+"\t"+isOthChars);
			// field.requestFocus();
		}
	}
	
	public void figureFunction (String s) {
		
		if (isOthChars==true) {
			otherCharacter = otherCharacter+s;
			field.setText(otherCharacter);
		}
		else if (isFstNum==false) {
			firstNum = s;
			isFstNum = true;
			field.setText(firstNum);
		}
		else if (isFstNum==true && isFirstOperator==false) {
			firstNum = firstNum+s;
			field.setText(firstNum);
		}
		else if (isSecNum==false) {
			secondNum = s;
			isSecNum = true;
			field.setText(firstNum+firstOperator+secondNum);
		}
		else if (isSecNum==true) {
			secondNum = secondNum+s;
			field.setText(firstNum+firstOperator+secondNum);
		}
		System.out.println(isFstNum+"\t"+isFirstOperator+"\t"+isSecNum+"\t"+isOthChars);
		// field.requestFocus();
	}
	
	public void operaterFunction(String s) {
		if (isOthChars==true){
			otherCharacter = otherCharacter+s;
			field.setText(otherCharacter);
		}
		else if (isSecNum==true && isSecondOperator==false) {		//第二个数为true，前面的数都是正确的输入
			secondOperator = s;
			isSecondOperator = true;
			calculate();
		}
		else if (isFstNum==true && isFirstOperator==false) {		//确定这是第一个操作符，而不是其他的什么东西
			firstOperator = s;
			isFirstOperator = true;
			field.setText(firstNum+firstOperator);
		}
		else if (isFstNum==true && isFirstOperator==true) {		
			if (isOthChars==false) {			//firstNum和firstOperator都有，otherChars没有，附加一个上去
				otherCharacter = firstNum+firstOperator+s;
				isOthChars = true;
			}
			else if (isOthChars==true) {		//firstNum和firstOperator都有，otherChars也有，就附加一个上去
				otherCharacter = otherCharacter+s;
			}
			field.setText(otherCharacter);
		}
		System.out.println(isFstNum+"\t"+isFirstOperator+"\t"+isSecNum+"\t"+isOthChars);
		// field.requestFocus();
	}
	
	class TopBut2Listener implements ActionListener {		//÷按钮
		public void actionPerformed (ActionEvent event) {
			operaterFunction("÷");
		}
	}
	
	//第二行的四个按钮
	class midBut0Listener implements ActionListener {		//数字7按钮
		public void actionPerformed(ActionEvent event) {
			figureFunction("7");
		}
	}
	class midBut1Listener implements ActionListener {		//数字8按钮
		public void actionPerformed(ActionEvent event) {
			figureFunction("8");
		}
	}
	class midBut2Listener implements ActionListener {		//数字9按钮
		public void actionPerformed(ActionEvent event) {
			figureFunction("9");
		}
	}
	class midBut3Listener implements ActionListener {		//*按钮
		public void actionPerformed(ActionEvent event) {
			operaterFunction("*");
		}
	}
	
		//第三行的四个按钮
	class midBut4Listener implements ActionListener {		//数字4按钮
		public void actionPerformed(ActionEvent event) {
			figureFunction("4");
		}
	}
	class midBut5Listener implements ActionListener {		//数字5按钮
		public void actionPerformed(ActionEvent event) {
			figureFunction("5");
		}
	}
	class midBut6Listener implements ActionListener {		//数字6按钮
		public void actionPerformed(ActionEvent event) {
			figureFunction("6");
		}
	}
	class midBut7Listener implements ActionListener {		//-按钮
		public void actionPerformed(ActionEvent event) {
			operaterFunction("-");
		}
	}
	
		//第四行的四个按钮
	class midBut8Listener implements ActionListener {		//数字1按钮
		public void actionPerformed(ActionEvent event) {
			figureFunction("1");
		}
	}
	class midBut9Listener implements ActionListener {		//数字2按钮
		public void actionPerformed(ActionEvent event) {
			figureFunction("2");
		}
	}
	class midBut10Listener implements ActionListener {		//数字3按钮
		public void actionPerformed(ActionEvent event) {
			figureFunction("3");
		}
	}
	class midBut11Listener implements ActionListener {		//-按钮
		public void actionPerformed(ActionEvent event) {
			operaterFunction("+");
		}
	}
	
	//第五行的三个按钮
	class botBut0Listener implements ActionListener {		//数字0按钮
		public void actionPerformed(ActionEvent event) {
			figureFunction("0");
		}
	}
	class botBut1Listener implements ActionListener {		//.按钮
		public void actionPerformed(ActionEvent event) {
			
			if (isOthChars==true) {
				otherCharacter = otherCharacter+".";
				field.setText(otherCharacter);
			}
			else if (isFstNum==true && isFirstOperator==false) {
				isFstDot = true;
				firstNum = firstNum+".";
				field.setText(firstNum);
			}
			else if (isSecNum==true && isSecondOperator==false) {
				isSecDot = true;
				secondNum = secondNum+".";
				field.setText(firstNum+firstOperator+secondNum);
			}
			else if (isFstDot==true && isOthChars==false) {
				isOthChars = true;
				otherCharacter = firstNum+".";
				field.setText(otherCharacter);
			}
			System.out.println(isFstNum+"\t"+isFirstOperator+"\t"+isSecNum+"\t");
			// field.requestFocus();
		}
	}
	
	
		
	class botBut2Listener implements ActionListener {		//=按钮
		public void actionPerformed(ActionEvent event) {
			operaterFunction("=");
		}
	}
}
