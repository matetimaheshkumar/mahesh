package javaProject1;

import java.awt.EventQueue;


import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;

import java.awt.Font;
import javax.swing.JTextField;
import javax.swing.JPasswordField;
import javax.swing.JButton;
import java.awt.event.ActionListener;
import java.sql.*;
import java.awt.event.ActionEvent;
import javax.swing.SwingConstants;
import java.awt.Color;

public class CustomerLogin {

	private JFrame frame;
	private JTextField txtUname;
	private JPasswordField txtpwd;
	private String Uname,pwd;
	/**
	 * Launch the application.
	 */
	public static void main(String[] args) {
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					CustomerLogin window = new CustomerLogin();
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
	public CustomerLogin() {
		initialize();
		frame.setVisible(true);
	}

	/**
	 * Initialize the contents of the frame.
	 */
	private void initialize() {
		frame = new JFrame();
		frame.setBounds(100, 100, 1420, 832);
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		frame.getContentPane().setLayout(null);
		
		JLabel lblNewLabel = new JLabel("UserName:");
		lblNewLabel.setHorizontalAlignment(SwingConstants.CENTER);
		lblNewLabel.setFont(new Font("Tahoma", Font.BOLD, 18));
		lblNewLabel.setBounds(448, 123, 218, 44);
		frame.getContentPane().add(lblNewLabel);
		
		txtUname = new JTextField();
		txtUname.setBounds(633, 135, 211, 28);
		frame.getContentPane().add(txtUname);
		txtUname.setColumns(10);
		
		JLabel lblNewLabel_1 = new JLabel("Password :");
		lblNewLabel_1.setHorizontalAlignment(SwingConstants.CENTER);
		lblNewLabel_1.setFont(new Font("Tahoma", Font.BOLD, 18));
		lblNewLabel_1.setBounds(479, 164, 160, 33);
		frame.getContentPane().add(lblNewLabel_1);
		
		txtpwd = new JPasswordField();
		txtpwd.setBounds(633, 170, 211, 28);
		frame.getContentPane().add(txtpwd);
		
		JButton btnLogin = new JButton("Login");
		btnLogin.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				try {
					Class.forName("com.mysql.cj.jdbc.Driver");
					Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/user_register", "root", "");
					Statement stmt = con.createStatement();
					Uname = txtUname.getText();
					pwd = String.valueOf(txtpwd.getPassword());
					String qry = "select * from userregister where UserName ='"+Uname+"'";
					ResultSet rs = stmt.executeQuery(qry);
					String dbUname=null, dbName=null,dbpwd = null;
					if(rs.next()) {
						dbUname = rs.getString("UserName");
						dbName = rs.getString("FullName");
						dbpwd = rs.getString("password");
					}
					if(Uname.equalsIgnoreCase(dbUname) && pwd.equals(dbpwd)) {
					//System.out.println(dbName);
						frame.dispose();
						Restaurant res = new Restaurant();
						res.setVisible(true);
						
					}
					else {
						JOptionPane.showMessageDialog(btnLogin, "Invalid UserName/Password");
					}
				}
				catch(Exception ex) {ex.printStackTrace(); }
			}
		});
		btnLogin.setFont(new Font("Tahoma", Font.BOLD, 12));
		btnLogin.setBounds(571, 254, 116, 28);
		frame.getContentPane().add(btnLogin);
		
		JButton btnNewButton_2 = new JButton("←");
		btnNewButton_2.setForeground(new Color(0, 0, 0));
		btnNewButton_2.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				frame.dispose();
				loginpage log = new loginpage();
			}
		});
		btnNewButton_2.setFont(new Font("Tahoma", Font.BOLD, 18));
		btnNewButton_2.setBounds(17, 10, 92, 28);
		frame.getContentPane().add(btnNewButton_2);
		
		JButton btnNewButton_1 = new JButton("Cancel");
		btnNewButton_1.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				txtUname.setText(null);
				txtpwd.setText(null);
			}
		});
		btnNewButton_1.setFont(new Font("Tahoma", Font.BOLD, 12));
		btnNewButton_1.setBounds(716, 254, 128, 28);
		frame.getContentPane().add(btnNewButton_1);
		
		JLabel lblNewLabel_2 = new JLabel("Customer login page");
		lblNewLabel_2.setHorizontalAlignment(SwingConstants.CENTER);
		lblNewLabel_2.setFont(new Font("Tahoma", Font.BOLD, 21));
		lblNewLabel_2.setBounds(469, 46, 412, 44);
		frame.getContentPane().add(lblNewLabel_2);
		
		JLabel lblNewLabel_3 = new JLabel("New User ?");
		lblNewLabel_3.setHorizontalAlignment(SwingConstants.CENTER);
		lblNewLabel_3.setFont(new Font("Tahoma", Font.BOLD, 16));
		lblNewLabel_3.setBounds(555, 321, 148, 28);
		frame.getContentPane().add(lblNewLabel_3);
		
		JButton btnNewButton_3 = new JButton("SignUp");
		btnNewButton_3.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				frame.dispose();
				SignUpPage sup = new SignUpPage();
			}
		});
		btnNewButton_3.setFont(new Font("Tahoma", Font.BOLD, 12));
		btnNewButton_3.setBounds(696, 322, 116, 28);
		frame.getContentPane().add(btnNewButton_3);
	}
}
