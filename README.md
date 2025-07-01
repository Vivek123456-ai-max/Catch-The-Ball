import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.Random;

public class CatchTheBallGame extends JPanel implements ActionListener, KeyListener {
    private int ballX, ballY;
    private int basketX;
    private int score = 0;
    private Timer timer;
    private boolean leftPressed, rightPressed;

    public CatchTheBallGame() {
        setPreferredSize(new Dimension(400, 600));
        setBackground(Color.BLACK);
        setFocusable(true);
        addKeyListener(this);

        ballX = new Random().nextInt(350);
        ballY = 0;
        basketX = 150;

        timer = new Timer(20, this); // 50 FPS
        timer.start();
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        // Move basket
        if (leftPressed && basketX > 0) basketX -= 5;
        if (rightPressed && basketX < 330) basketX += 5;

        // Move ball
        ballY += 5;

        // Check for catch
        if (ballY >= 530 && ballY <= 550 && ballX + 20 >= basketX && ballX <= basketX + 70) {
            score++;
            ballY = 0;
            ballX = new Random().nextInt(350);
        }

        // Missed ball
        if (ballY > 600) {
            ballY = 0;
            ballX = new Random().nextInt(350);
            score = 0; // Reset score on miss
        }

        repaint();
    }

    @Override
    protected void paintComponent(Graphics g) {
        super.paintComponent(g);

        // Draw ball
        g.setColor(Color.RED);
        g.fillOval(ballX, ballY, 20, 20);

        // Draw basket
        g.setColor(Color.GREEN);
        g.fillRect(basketX, 550, 70, 20);

        // Draw score
        g.setColor(Color.WHITE);
        g.setFont(new Font("Arial", Font.BOLD, 20));
        g.drawString("Score: " + score, 10, 25);
    }

    // Key listeners
    public void keyPressed(KeyEvent e) {
        if (e.getKeyCode() == KeyEvent.VK_LEFT) leftPressed = true;
        if (e.getKeyCode() == KeyEvent.VK_RIGHT) rightPressed = true;
    }

    public void keyReleased(KeyEvent e) {
        if (e.getKeyCode() == KeyEvent.VK_LEFT) leftPressed = false;
        if (e.getKeyCode() == KeyEvent.VK_RIGHT) rightPressed = false;
    }

    public void keyTyped(KeyEvent e) {}

    public static void main(String[] args) {
        JFrame frame = new JFrame("Catch the Ball");
        CatchTheBallGame game = new CatchTheBallGame();

        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.add(game);
        frame.pack();
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);
    }
}
