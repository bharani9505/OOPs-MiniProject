# OOPs-MiniProject
package AuctionBit;
public class AuctionItem {
    private String itemName;
    private double highestBid;
    private String highestBidder;

    public AuctionItem(String itemName) {
        this.itemName = itemName;
        this.highestBid = 0.0;
        this.highestBidder = "No bids yet";
    }

    public String getItemName() {
        return itemName;
    }

    public double getHighestBid() {
        return highestBid;
    }

    public String getHighestBidder() {
        return highestBidder;
    }

    public boolean placeBid(String bidder, double bidAmount) {
        if (bidAmount > highestBid) {
            highestBid = bidAmount;
            highestBidder = bidder;
            return true;
        }
        return false;
    }
}
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class AuctionSystemGUI {

    private JFrame frame;
    private JTextField itemField, bidderField, bidField;
    private JTextArea outputArea;

    private AuctionItem auctionItem;

    public AuctionSystemGUI() {

        frame = new JFrame("Simple Auction System");
        frame.setSize(500, 500);
        frame.setLayout(null);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        JLabel title = new JLabel("Auction System");
        title.setFont(new Font("Arial", Font.BOLD, 22));
        title.setBounds(160, 10, 200, 30);
        frame.add(title);

        JLabel itemLabel = new JLabel("Item:");
        itemLabel.setBounds(20, 60, 120, 30);
        frame.add(itemLabel);

        itemField = new JTextField();
        itemField.setBounds(150, 60, 200, 30);
        frame.add(itemField);

        JButton createBtn = new JButton("Create Auction");
        createBtn.setBounds(150, 100, 200, 30);
        frame.add(createBtn);

        JLabel bidderLabel = new JLabel("Bidder Name:");
        bidderLabel.setBounds(20, 150, 120, 30);
        frame.add(bidderLabel);

        bidderField = new JTextField();
        bidderField.setBounds(150, 150, 200, 30);
        frame.add(bidderField);

        JLabel bidLabel = new JLabel("Bid Amount:");
        bidLabel.setBounds(20, 190, 120, 30);
        frame.add(bidLabel);

        bidField = new JTextField();
        bidField.setBounds(150, 190, 200, 30);
        frame.add(bidField);

        JButton bidBtn = new JButton("Place Bid");
        bidBtn.setBounds(150, 230, 200, 30);
        frame.add(bidBtn);

        JButton showWinnerBtn = new JButton("Show Winner");
        showWinnerBtn.setBounds(150, 270, 200, 30);
        frame.add(showWinnerBtn);

        outputArea = new JTextArea();
        outputArea.setEditable(false);

        JScrollPane scroll = new JScrollPane(outputArea);
        scroll.setBounds(20, 320, 440, 130);
        frame.add(scroll);

        
        createBtn.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                String itemName = itemField.getText();
                if (itemName.equals("")) {
                    JOptionPane.showMessageDialog(frame, "Please enter item name!");
                    return;
                }

                auctionItem = new AuctionItem(itemName);
                outputArea.setText("Auction created for item: " + itemName + "\n");
            }
        });

        bidBtn.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {

                if (auctionItem == null) {
                    JOptionPane.showMessageDialog(frame, "Create auction first!");
                    return;
                }

                String bidder = bidderField.getText();
                String bidStr = bidField.getText();

                if (bidder.equals("") || bidStr.equals("")) {
                    JOptionPane.showMessageDialog(frame, "Enter bidder and amount!");
                    return;
                }

                double bidAmount;

                try {
                    bidAmount = Double.parseDouble(bidStr);
                } catch (Exception ex) {
                    JOptionPane.showMessageDialog(frame, "Invalid bid amount!");
                    return;
                }

                boolean success = auctionItem.placeBid(bidder, bidAmount);

                if (success) {
                    outputArea.append("New Highest Bid: " + bidder + " → ₹" + bidAmount + "\n");
                } else {
                    outputArea.append("Bid too low by " + bidder + "\n");
                }
            }
        });

        showWinnerBtn.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {

                if (auctionItem == null) {
                    JOptionPane.showMessageDialog(frame, "No auction created yet!");
                    return;
                }

                outputArea.append("\n--- WINNER ---\n");
                outputArea.append("Item: " + auctionItem.getItemName() + "\n");
                outputArea.append("Winner: " + auctionItem.getHighestBidder() + "\n");
                outputArea.append("Winning Bid: ₹" + auctionItem.getHighestBid() + "\n");
            }
        });

        frame.setVisible(true);
    }

    public static void main(String[] args) {
        new AuctionSystemGUI();
    }
}
