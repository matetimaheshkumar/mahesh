package javaProject1;

import java.awt.Dimension;
import java.awt.EventQueue;
import java.awt.Font;

import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.border.EmptyBorder;
import javax.swing.table.DefaultTableModel;

public class PrintBill extends JFrame {

    private static final long serialVersionUID = 1L;
    private JPanel contentPane;
    private JTextArea textAreaBill;

    public PrintBill(DefaultTableModel orderTableModel) {
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setBounds(100, 100, 734, 469);
        contentPane = new JPanel();
        contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
        setContentPane(contentPane);
        contentPane.setLayout(null);

        JScrollPane scrollPane = new JScrollPane();
        scrollPane.setBounds(12, 10, 707, 406);
        contentPane.add(scrollPane);

        textAreaBill = new JTextArea();
        textAreaBill.setEditable(false);
        scrollPane.setViewportView(textAreaBill);

        Font headingFont = new Font("Arial", Font.BOLD, 20);
        textAreaBill.setFont(headingFont);
        
        textAreaBill.append("		Epicurean Haven\n\n");
        textAreaBill.append("================================================\n");
        
        textAreaBill.append("ID	Dish 	Price	Quantity\n");
        
        textAreaBill.append("\n\n");

        for (int row = 0; row < orderTableModel.getRowCount(); row++) {
            String orderID = orderTableModel.getValueAt(row, 0).toString();
            String dish = orderTableModel.getValueAt(row, 1).toString();
            String price = orderTableModel.getValueAt(row, 2).toString();
            String quantity = orderTableModel.getValueAt(row, 3).toString();

            textAreaBill.append(String.format("%-8s %-30s %s %s\n", orderID, dish, price, quantity));
        }

        double total = calculateTotal(orderTableModel);
        textAreaBill.append("\n\n=================================================" +
                "\n\t\t\tTotal Amount:-"+total+
                "\n================       VISIT US AGAIN!   ================");

        Dimension preferredSize = new Dimension(707, 406);
        scrollPane.setPreferredSize(preferredSize);
    }

    private double calculateTotal(DefaultTableModel orderTableModel) {
        double total = 0.0;
        for (int row = 0; row < orderTableModel.getRowCount(); row++) {
            double price = Double.parseDouble(orderTableModel.getValueAt(row, 2).toString());
            int quantity = Integer.parseInt(orderTableModel.getValueAt(row, 3).toString());
            total += price * quantity;
        }
        return total;
    }

    public static void main(String[] args) {
        EventQueue.invokeLater(new Runnable() {
            public void run() {
                try {
                    // For testing
                    DefaultTableModel orderTableModel = new DefaultTableModel();
                    orderTableModel.addColumn("Order ID");
                    orderTableModel.addColumn("Dish");
                    orderTableModel.addColumn("Price");
                    orderTableModel.addColumn("Quantity");
                    
                    PrintBill printBill = new PrintBill(orderTableModel);
                    printBill.setVisible(true);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        });
    }
}
