# swing画图延迟替代方法

课程需要爆肝了一天swing的画图部分，之前在python上用过QT，有联系有不同吧。就是很简单实现一个画板的功能，真·萌新。不对的地方请指出。

先吐槽下javafx....第一次用java搞GUI，虽然javafx比swing更新功能更全。。但是打jar包这个步骤拿捏的死死的，相比来说教程也很少（JAVA果然不适合搞UI）。。。java11中被移出了jdk，不支持maven打包劝退。

言归正传，画图这里有两种逻辑，第一种是直接在一张图上画，所有的操作都在这个画布上完成，每次更新画布，另一种是记录每次画的对象比如直线等，每次重画这些对象就可以达到更新的作用

## PlanA

```java
public class lagTest {
    JFrame frame;
    JPanel jPanel;
    Graphics2D g;

    int x = -1;
    int y = -1;

    public lagTest() {
        frame = new JFrame();
        jPanel = new JPanel();

        frame.setTitle("test");
        frame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        frame.setSize(1024, 1024);

        frame.add(jPanel);
        frame.setVisible(true);
        System.out.println(jPanel.getGraphics());
        g = (Graphics2D) jPanel.getGraphics();
        g.setColor(Color.WHITE);// set the color for drawing
        g.fillRect(0, 0, 1024, 1024);//set background
        g.setColor(Color.BLACK);
        
        jPanel.addMouseMotionListener(new MouseMotionAdapter() {
            @Override
            public void mouseDragged(MouseEvent e) {
                if (x > 0 && y > 0) {
                    System.out.println("im in");
                    BasicStroke bStroke = new BasicStroke(3, BasicStroke.CAP_ROUND, BasicStroke.JOIN_ROUND);
                    g.setStroke(bStroke);
                    g.setColor(Color.black);
                    g.drawLine(x, y, e.getX(), e.getY());
                }
                x = e.getX();
                y = e.getY();
                //jPanel.repaint();

            }
        });
        jPanel.addMouseListener(new MouseAdapter() {
            @Override
            public void mouseReleased(MouseEvent e) {
                x = -1;
                y = -1;
            }
        });


    }

    public static void main(String[] args) {
        lagTest test = new lagTest();

    }
}
```

 这个方法属于直接在jpanel上画，getGraphics\(\)得到的是jpanel的图形对象（图形对象就是个工具，可以在这个图形上操作画画啥的），这里面注意jpanel的getGraphics\(\)一定要在jpanel被添加进Jframe以及jframe setVisible之后，否则会返回null造成空指针异常。但是测试的时候发现画画的延迟很大不知道为啥🤷‍♂️，尝试每次调用jpanel.repaint增加频率直接不显示了，文档说repaint会直接去调用paint或者update，但是怎么重写paint内容不会= =。。。大佬知道啥情况的话请告诉我。。。

## PlanB

```java
public class lagTest {
    JFrame frame;
    JPanel jPanel;
    Image img;
    Graphics2D g;

    int x = -1;
    int y = -1;

    public lagTest() {
        frame = new JFrame();
        jPanel = new JPanel(){
            @Override
            public void paint(Graphics g1) {
                g1.drawImage(img, 0, 0, null);
            }
        };

        img = new BufferedImage(1024, 1024, BufferedImage.TYPE_INT_BGR);
        g = (Graphics2D) img.getGraphics();

        frame.setTitle("test");
        frame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        frame.setSize(1024, 1024);

        g.setColor(Color.WHITE);// set the color for drawing
        g.fillRect(0, 0, 1024, 1024);//set background
        g.setColor(Color.BLACK);
        frame.add(jPanel);

        frame.setVisible(true);

        jPanel.addMouseMotionListener(new MouseMotionAdapter() {
            @Override
            public void mouseDragged(MouseEvent e) {
                if (x > 0 && y > 0) {
                    System.out.println("im in");
                    BasicStroke bStroke = new BasicStroke(3, BasicStroke.CAP_ROUND, BasicStroke.JOIN_ROUND);
                    g.setStroke(bStroke);
                    g.setColor(Color.black);
                    g.drawLine(x, y, e.getX(), e.getY());
                }
                x = e.getX();
                y = e.getY();
                jPanel.repaint();

            }
        });
        jPanel.addMouseListener(new MouseAdapter() {
            @Override
            public void mouseReleased(MouseEvent e) {
                x = -1;
                y = -1;
            }
        });

    }

    public static void main(String[] args) {
        lagTest test = new lagTest();

    }
}
```

 借助一个img对象每次在上面操作，然后repaint，这样下来就很流畅

## 补充：问题解决！在planA也可以无延迟刷新

之前白了的原因在于repaint时候没有考虑到之前的点。。所以每次画都只有一个点。。加一个数组存储之前的点就好

```java
public class DraPictureCanvas {
    JFrame frame;
    JPanel jPanel;
    Graphics2D g;
    ArrayList<int[]> line = new ArrayList<>();

    int x = -1;
    int y = -1;


    public DraPictureCanvas() {
        frame = new JFrame();
        jPanel = new JPanel() {
            @Override
            public void paint(Graphics g1) {
                for (int[] position : line) {
                    Graphics2D g2D = (Graphics2D)g1;
                    System.out.println(Arrays.toString(position));
                    BasicStroke bStroke = new BasicStroke(3, BasicStroke.CAP_ROUND, BasicStroke.JOIN_ROUND);
                    g2D.setStroke(bStroke);
                    g2D.setColor(Color.black);
                    g2D.drawLine(position[0], position[1], position[2], position[3]);
                }
            }

        };

        frame.setTitle("test");
        frame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        frame.setSize(1024, 1024);

        frame.add(jPanel);
        frame.setVisible(true);
        System.out.println(jPanel.getGraphics());
        g = (Graphics2D) jPanel.getGraphics();
        g.setColor(Color.WHITE);// set the color for drawing
        g.fillRect(0, 0, 1024, 1024);//set background
        g.setColor(Color.BLACK);

        jPanel.addMouseMotionListener(new MouseMotionAdapter() {
            @Override
            public void mouseDragged(MouseEvent e) {
                if (x > 0 && y > 0) {
                    BasicStroke bStroke = new BasicStroke(3, BasicStroke.CAP_ROUND, BasicStroke.JOIN_ROUND);
                    g.setStroke(bStroke);
                    g.setColor(Color.black);
                    g.drawLine(x, y, e.getX(), e.getY());
                   // System.out.println(x+" "+y+" "+e.getX()+" "+e.getY());
                    line.add(new int[]{x, y, e.getX(), e.getY()});
                }
                jPanel.repaint();
                x = e.getX();
                y = e.getY();


            }
        });
        jPanel.addMouseListener(new MouseAdapter() {
            @Override
            public void mousePressed(MouseEvent e) {
                x = -1;
                y = -1;
            }
        });


    }

    public static void main(String[] args) {
        DraPictureCanvas test = new DraPictureCanvas();

    }
}
```

 



