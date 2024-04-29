package javaProject1;

import java.awt.EventQueue;

import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JLabel;
import javax.swing.SwingConstants;
import javax.swing.table.DefaultTableModel;

import java.awt.Font;
import javax.swing.JComboBox;
import java.awt.event.ActionListener;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Vector;
import java.awt.event.ActionEvent;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.JButton;
import javax.swing.JTextField;
import javax.swing.ImageIcon;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;

public class ResMenu {

	private JFrame frame;
	private JTextField textField;
	private JTextField textField_1;
	private JTextField textField_2;
	private JTable table;

	/**
	 * Launch the application.
	 */
	public static void main(String[] args) {
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					ResMenu window = new ResMenu();
					window.frame.setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}

	/**
	 * Create the application.
	 */
	public ResMenu() {
		initialize();
		frame.setVisible(true);
	}

	/**
	 * Initialize the contents of the frame.
	 */
	private void initialize() {
		frame = new JFrame();
		frame.setBounds(100, 100, 1252, 637);
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		frame.getContentPane().setLayout(null);
		
		 textField = new JTextField();
		 textField.setBounds(324, 121, 132, 33);
		 frame.getContentPane().add(textField);
		 textField.setColumns(10);
		
		 JScrollPane scrollPane = new JScrollPane();
		 scrollPane.setBounds(10, 292, 1218, 297);
		 frame.getContentPane().add(scrollPane);
		 
		 table = new JTable();
		 Object[] columns = {"ID","Dish","Price"};
			DefaultTableModel model = new DefaultTableModel();     
			model.setColumnIdentifiers(columns);
			table.setModel(model);
		 table.addMouseListener(new MouseAdapter() {
		 	@Override
		 	public void mouseClicked(MouseEvent e) {
		 		int i = table.getSelectedRow();
		 		textField.setText(model.getValueAt(i,0).toString());
		 		textField_1.setText(model.getValueAt(i,1).toString());
		 		textField_2.setText(model.getValueAt(i,2).toString());
		 	}
		 });
		 scrollPane.setViewportView(table);
		 
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
				DefaultTableModel models = (DefaultTableModel) table.getModel();
				models.setColumnIdentifiers(cols);
				while(rs.next()) {
					String[] row = {rs.getString(1),rs.getString(2),rs.getString(3)};
					models.addRow(row);
				}
			} catch (ClassNotFoundException | SQLException e1) {
				// TODO Auto-generated catch block
				e1.printStackTrace();
			}
		 
					 JButton btnNewButton = new JButton("Add");
					 btnNewButton.addActionListener(new ActionListener() {
					 	public void actionPerformed(ActionEvent e) {
					 	    try {
					            Class.forName("com.mysql.cj.jdbc.Driver");
					            Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/user_register", "root", "");
					            Statement stmt = con.createStatement();

					            String ID = textField.getText();
					            String Dish = textField_1.getText();
					            String Price = textField_2.getText();

					            // Fetch existing IDs from the database
					            String query = "SELECT ID FROM menu";
					            ResultSet rs = stmt.executeQuery(query);
					            if(textField.getText().equals(")|| textField_1.getText().equals(")|| textField_2.getText().equals("")){
									JOptionPane.showMessageDialog(null,"please fill the full information");
								}else {
					            while (rs.next()) {
					                String dbID = rs.getString("ID");
					                if (ID.equalsIgnoreCase(dbID)) {
					                    JOptionPane.showMessageDialog(frame, "ID already exists, please try again");
					                    return; // Exit the method if ID already exists
					                }
					            }

					            // Your existing code for adding to the menu
					            String sql = "INSERT INTO menu VALUES('" + ID + "', '" + Dish + "','" + Price + "')";
					            stmt.executeUpdate(sql);

					            // Refresh the table
					            String qry = "SELECT * FROM menu";
					            ResultSet rsRefresh = stmt.executeQuery(qry);
					            ResultSetMetaData rsmd = rsRefresh.getMetaData();
					            int n = rsmd.getColumnCount();
					            DefaultTableModel dtm = (DefaultTableModel) table.getModel();
					            dtm.setRowCount(0);
					            while (rsRefresh.next()) {
					                Vector<Object> v = new Vector<>();
					                for (int i = 1; i <= n; i++) {
					                    v.add(rsRefresh.getString("ID"));
					                    v.add(rsRefresh.getString("Dish"));
					                    v.add(rsRefresh.getString("Price"));
					                }
					                dtm.addRow(v);
					            }

					            JOptionPane.showMessageDialog(frame, "Registration Completed Successfully");
								}
					        } catch (Exception ex) {
					            ex.printStackTrace();
					        }
					 	}
					 });
					 btnNewButton.setFont(new Font("Tahoma", Font.PLAIN, 14));
					 btnNewButton.setBounds(421, 177, 154, 33);
					 frame.getContentPane().add(btnNewButton);
					 
					 JButton btnNewButton_1 = new JButton("update");
					 btnNewButton_1.addActionListener(new ActionListener() {
					 	public void actionPerformed(ActionEvent e) {
					 		int i = table.getSelectedRow();
					 		if(i>=0) {
					 	 		try {
									Class.forName("com.mysql.cj.jdbc.Driver");
									Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/user_register", "root", "");
									Statement stmt = con.createStatement();
									String ID = textField.getText();
									String Dish = textField_1.getText();
									String Price = textField_2.getText();
									if(textField.getText().equals(")|| textField_1.getText().equals(")|| textField_2.getText().equals("")){
										JOptionPane.showMessageDialog(null,"please fill the full information");
									}
									else {
									 String updateSQL = "UPDATE menu SET Dish='" + Dish + "', Price='" + Price + "' WHERE ID='" + ID + "'";
					                    stmt.executeUpdate(updateSQL);

					                    // Update the data in the table model
					                    model.setValueAt(ID, i, 0);
					                    model.setValueAt(Dish, i, 1);
					                    model.setValueAt(Price, i, 2);
									}
						 		}
									catch(Exception ex) {ex.printStackTrace(); }	
					 		JOptionPane.showMessageDialog(null, "updated successfully");
					 		}
					 		else {
					 			JOptionPane.showMessageDialog(null, "please select the row first");
					 		}
					 	}
					 });
					 btnNewButton_1.setFont(new Font("Tahoma", Font.PLAIN, 14));
					 btnNewButton_1.setBounds(605, 177, 154, 33);
					 frame.getContentPane().add(btnNewButton_1);
					 
					 JButton btnNewButton_2 = new JButton("Delete");
					 btnNewButton_2.addActionListener(new ActionListener() {
					 	public void actionPerformed(ActionEvent e) {
					 		  int selectedRow = table.getSelectedRow();
					 	        if (selectedRow >= 0) {
					 	            try {
					 	                String ID = model.getValueAt(selectedRow, 0).toString();

					 	                Class.forName("com.mysql.cj.jdbc.Driver");
					 	                Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/user_register", "root", "");
					 	                Statement stmt = con.createStatement();
					 	                String deleteSQL = "DELETE FROM menu WHERE ID='" + ID + "'";
					 	                stmt.executeUpdate(deleteSQL);
					 	                model.removeRow(selectedRow);
					 	                JOptionPane.showMessageDialog(null, "Deleted successfully");
					 	            } catch (Exception ex) {
					 	                ex.printStackTrace();
					 	            }
					 	        } else {
					 	            JOptionPane.showMessageDialog(null, "Please select a row first");
					 	        }
					 		
					 	}
					 });
					 btnNewButton_2.setFont(new Font("Tahoma", Font.PLAIN, 14));
					 btnNewButton_2.setBounds(425, 238, 154, 33);
					 frame.getContentPane().add(btnNewButton_2);
					 
					 JButton btnNewButton_3 = new JButton("Clear");
					 btnNewButton_3.addActionListener(new ActionListener() {
					 	public void actionPerformed(ActionEvent e) {
					 		textField.setText(null);
					 		textField_1.setText(null);
					 		textField_2.setText(null);
					 	}
					 });
					 btnNewButton_3.setFont(new Font("Tahoma", Font.PLAIN, 14));
					 btnNewButton_3.setBounds(609, 238, 154, 33);
					 frame.getContentPane().add(btnNewButton_3);
					 

					 
					 JLabel lblNewLabel_1 = new JLabel("ID:");
					 lblNewLabel_1.setHorizontalAlignment(SwingConstants.CENTER);
					 lblNewLabel_1.setFont(new Font("Tahoma", Font.PLAIN, 16));
					 lblNewLabel_1.setBounds(249, 119, 115, 33);
					 frame.getContentPane().add(lblNewLabel_1);
					 
					 JLabel lblNewLabel_2 = new JLabel("Dish:");
					 lblNewLabel_2.setFont(new Font("Tahoma", Font.PLAIN, 16));
					 lblNewLabel_2.setHorizontalAlignment(SwingConstants.CENTER);
					 lblNewLabel_2.setBounds(436, 121, 123, 33);
					 frame.getContentPane().add(lblNewLabel_2);
					 
					 textField_1 = new JTextField();
					 textField_1.setText("");
					 textField_1.setBounds(523, 121, 182, 33);
					 frame.getContentPane().add(textField_1);
					 textField_1.setColumns(10);
					 
					 JLabel lblNewLabel_3 = new JLabel("Price:");
					 lblNewLabel_3.setHorizontalAlignment(SwingConstants.CENTER);
					 lblNewLabel_3.setFont(new Font("Tahoma", Font.PLAIN, 16));
					 lblNewLabel_3.setBounds(702, 119, 102, 33);
					 frame.getContentPane().add(lblNewLabel_3);
					 
					 textField_2 = new JTextField();
					 textField_2.setBounds(783, 122, 154, 32);
					 frame.getContentPane().add(textField_2);
					 textField_2.setColumns(10);
		 
		 
		 
		 
		 JButton btnNewButton_5 = new JButton("");
		 btnNewButton_5.addActionListener(new ActionListener() {
		 	public void actionPerformed(ActionEvent e) {
					int option = JOptionPane.showConfirmDialog(null, "Are you sure you want to log out?", "Confirmation", JOptionPane.YES_NO_OPTION);
	        		if(option == JOptionPane.YES_OPTION) {
	        		frame.dispose();
	        		RestaurantLogin cu = new RestaurantLogin();
	        		}
	        		else {
	        			//It Does'nt change nothing
	        		}
		 	}
		 	});
		 btnNewButton_5.setIcon(new ImageIcon("C:\\Users\\nikil\\Downloads\\icons8-logout-50.png"));
		 btnNewButton_5.setBounds(1153, 23, 62, 59);
		 frame.getContentPane().add(btnNewButton_5);
		 
		 JLabel lblNewLabel_4 = new JLabel("Menu Management");
		 lblNewLabel_4.setHorizontalAlignment(SwingConstants.CENTER);
		 lblNewLabel_4.setFont(new Font("Tahoma", Font.BOLD, 24));
		 lblNewLabel_4.setBounds(389, 23, 370, 72);
		 frame.getContentPane().add(lblNewLabel_4);
	}
}
