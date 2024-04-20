package javaProject1;

import java.awt.EventQueue;
import java.awt.Font;
import java.awt.ScrollPane;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Arrays;
import java.util.List;

import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.border.EmptyBorder;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.TableModel;
import javax.swing.JTable;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import javax.swing.JLabel;
import javax.swing.SwingConstants;
import javax.swing.JTabbedPane;

public class Menu extends JFrame {

    private static final long serialVersionUID = 1L;
    private JPanel contentPane;
    private JTable table;
    private DefaultTableModel orderTableModel;
    private int orderId = 1;
    /**
     * Launch the application.
     */
    public static void main(String[] args) {
        EventQueue.invokeLater(new Runnable() {
            public void run() {
                try {
                    Menu frame = new Menu(); 
                    frame.setVisible(true);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        });
    }
    private int getSelectedRowIndex() {
        return table.getSelectedRow();
    }

    private String getSelectedRowData() {
        int rowIndex = getSelectedRowIndex();
        if (rowIndex != -1) {
            TableModel model = table.getModel();
            int columnCount = model.getColumnCount();
            StringBuilder rowData = new StringBuilder();

            for (int i = 0; i < columnCount; i++) {
                Object value = model.getValueAt(rowIndex, i);
                rowData.append(value).append("\t");
            }
            rowData.append("\n");
            return rowData.toString();
        } else {
            return "No row selected.";
        }
    }
    /**
     * Create the frame.
     */
    public Menu() {
        setTitle("Menu");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setBounds(100, 100, 1356, 828);
        contentPane = new JPanel();
        contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));

        setContentPane(contentPane);
        contentPane.setLayout(null);

        JButton btnNewButton = new JButton("Home");
        btnNewButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                dispose();
                Restaurant rest = new Restaurant();
                rest.setVisible(true);
            }
        });
        btnNewButton.setFont(new Font("Arial", Font.BOLD | Font.ITALIC, 12));
        btnNewButton.setBounds(304, 10, 146, 32);
        contentPane.add(btnNewButton);

        JButton btnNewButton_1 = new JButton("About Us");
        btnNewButton_1.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                dispose();
                AboutUs AU = new AboutUs();
                AU.setVisible(true);
            }
        });
        btnNewButton_1.setFont(new Font("Arial", Font.BOLD | Font.ITALIC, 12));
        btnNewButton_1.setBounds(460, 10, 146, 32);
        contentPane.add(btnNewButton_1);
        
        JScrollPane scrollPane = new JScrollPane();
        scrollPane.setBounds(44, 101, 1165, 358);
        contentPane.add(scrollPane);
        
        table = new JTable();
        table.addMouseListener(new MouseAdapter() {
        	@Override
        	public void mouseClicked(MouseEvent e) {
        		int i = table.getSelectedRow();
        	}
        });
        scrollPane.setViewportView(table);
        
        ScrollPane scrollPane2 = new ScrollPane();
		scrollPane2.setVisible(true);
		try {
			Class.forName("com.mysql.cj.jdbc.Driver");
			Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/user_register","root","");
			Statement st = con.createStatement();
			ResultSet rs = st.executeQuery("select * from menu");
			ResultSetMetaData rmd = rs.getMetaData();
			int cc = rmd.getColumnCount();
			String cols[] = new String[cc];
			for(int i = 0; i< cc; i++)
				cols[i] = rmd.getColumnName(i+1);
			DefaultTableModel model = (DefaultTableModel) table.getModel();
			model.setColumnIdentifiers(cols);
			while(rs.next()) {
				String[] row = {rs.getString(1),rs.getString(2),rs.getString(3)};
				model.addRow(row);
			}
		} catch (ClassNotFoundException | SQLException e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		}

        JButton btnNewButton_2 = new JButton("Menu");
        btnNewButton_2.setFont(new Font("Arial", Font.BOLD | Font.ITALIC, 12));
        btnNewButton_2.setBounds(616, 10, 132, 32);
        contentPane.add(btnNewButton_2);

		JButton btnNewButton_3 = new JButton("Reviews");
		btnNewButton_3.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				dispose();
				Reviews rev = new Reviews();
				rev.setVisible(true);
			}
		});
		btnNewButton_3.setFont(new Font("Arial", Font.BOLD | Font.ITALIC, 12));
        btnNewButton_3.setBounds(758, 10, 132, 32);
        contentPane.add(btnNewButton_3);

		JButton btnNewButton_4 = new JButton("Contact us");
		btnNewButton_4.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				dispose();
				ContactUs conu = new ContactUs();
				conu.setVisible(true);
			}
		});
		 btnNewButton_4.setFont(new Font("Arial", Font.BOLD | Font.ITALIC, 12));
	        btnNewButton_4.setBounds(900, 10, 146, 32);
	        contentPane.add(btnNewButton_4);
	        
	        JButton btnNewButton_5 = new JButton("");
	        btnNewButton_5.addActionListener(new ActionListener() {
	        	public void actionPerformed(ActionEvent e) {
	        		int option = JOptionPane.showConfirmDialog(null, "Are you sure you want to log out?", "Confirmation", JOptionPane.YES_NO_OPTION);
	        		if(option == JOptionPane.YES_OPTION) {
	        		dispose();
	        		CustomerLogin cu = new CustomerLogin();
	        		}
	        		else {
	        			
	        		}
	        	}
	        });
	        btnNewButton_5.setIcon(new ImageIcon("C:\\Users\\nikil\\Downloads\\icons8-logout-50.png"));
	        btnNewButton_5.setBounds(1272, 10, 62, 59);
	        contentPane.add(btnNewButton_5);
	        
	        JLabel lblNewLabel_1 = new JLabel("MENU");
	        lblNewLabel_1.setHorizontalAlignment(SwingConstants.CENTER);
	        lblNewLabel_1.setFont(new Font("Tahoma", Font.BOLD, 20));
	        lblNewLabel_1.setBounds(500, 56, 207, 32);
	        contentPane.add(lblNewLabel_1);
	        
	        JScrollPane orderScrollPane = new JScrollPane();
	        orderScrollPane.setBounds(44, 503, 803, 260);
	        contentPane.add(orderScrollPane);
	        
	        orderTableModel = new DefaultTableModel();
	        orderTableModel.addColumn("Order ID");
	        orderTableModel.addColumn("Dish");
	        orderTableModel.addColumn("Price");
	        orderTableModel.addColumn("Quantity");

	        // Create a JTable with the DefaultTableModel
	        JTable orderTable = new JTable(orderTableModel);
	        orderTable.addMouseListener(new MouseAdapter() {
	        	@Override
	        	public void mouseClicked(MouseEvent e) {
	        		int i = orderTable.getSelectedRow();
	        	}
	        });
	        orderScrollPane.setViewportView(orderTable);
	        
	        JButton btnNewButton_3_1 = new JButton("ADD");
	        btnNewButton_3_1.addActionListener(new ActionListener() {
	            public void actionPerformed(ActionEvent e) {
	                addToOrder();
	            }
	        });
	        btnNewButton_3_1.setFont(new Font("Arial", Font.BOLD | Font.ITALIC, 12));
	        btnNewButton_3_1.setBounds(1076, 469, 178, 32);
	        contentPane.add(btnNewButton_3_1);
	        
	        JLabel lblNewLabel = new JLabel("Ordered Items");
	        lblNewLabel.setHorizontalAlignment(SwingConstants.CENTER);
	        lblNewLabel.setFont(new Font("Tahoma", Font.BOLD, 20));
	        lblNewLabel.setBounds(291, 469, 216, 25);
	        contentPane.add(lblNewLabel);
	        
	        JButton btnNewButton_3_1_1 = new JButton("Print Bill");
	        btnNewButton_3_1_1.addActionListener(new ActionListener() {
	        	public void actionPerformed(ActionEvent e) {
	        		if (orderTableModel.getRowCount() == 0) {
	                    JOptionPane.showMessageDialog(null, "No dish selected,Please select the Dish First to print bill", "Error", JOptionPane.ERROR_MESSAGE);
	                } else {
	                    PrintBill printBillPage = new PrintBill(orderTableModel);
	                    printBillPage.setVisible(true);
	                }
	        	}
	        });
	        btnNewButton_3_1_1.setFont(new Font("Arial", Font.BOLD | Font.ITALIC, 12));
	        btnNewButton_3_1_1.setBounds(918, 659, 218, 51);
	        contentPane.add(btnNewButton_3_1_1);
	        
	        JButton btnNewButton_3_1_1_1 = new JButton("Delete");
	        btnNewButton_3_1_1_1.addActionListener(new ActionListener() {
	        	public void actionPerformed(ActionEvent e) {
	        		 int selectedRow = orderTable.getSelectedRow();
	        	        if (selectedRow >= 0) {
	        	        orderTableModel.removeRow(selectedRow);
	        	        }else {JOptionPane.showMessageDialog(btnNewButton_3_1_1_1, "Please select the row first");}
	        		
	        	}
	        });
	        btnNewButton_3_1_1_1.setFont(new Font("Arial", Font.BOLD | Font.ITALIC, 12));
	        btnNewButton_3_1_1_1.setBounds(918, 560, 218, 51);
	        contentPane.add(btnNewButton_3_1_1_1);
    }
    private void addToOrder() {
        int selectedRow = table.getSelectedRow();
        if (selectedRow >= 0) {
            int quantity = getQuantityFromUser();
            if (quantity >= 0) {
                String dish = table.getValueAt(selectedRow, 1).toString();
                String price = table.getValueAt(selectedRow, 2).toString();
                String[] rowData = { String.valueOf(orderId), dish, price, String.valueOf(quantity) };
                orderTableModel.addRow(rowData);
                orderId++; // Increment order ID for the next order
            }
        }
    }

    private int getQuantityFromUser() {
        String quantityStr = JOptionPane.showInputDialog("Enter Quantity:");
        try {
            return Integer.parseInt(quantityStr);
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(null, "Invalid input. Please enter a valid quantity.", "Error", JOptionPane.ERROR_MESSAGE);
            return -1;
        }
    }
}
