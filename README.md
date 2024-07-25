# Contact-Database-Management-System
This code provides a basic Contact Database Management System using a Binary Search Tree in Java, with functionality to insert, search, and delete contacts.


class Contact {
    String name;
    String phoneNumber;
    String email;

    public Contact(String name, String phoneNumber, String email) {
        this.name = name;
        this.phoneNumber = phoneNumber;
        this.email = email;
    }

    @Override
    public String toString() {
        return "Name: " + name + ", Phone: " + phoneNumber + ", Email: " + email;
    }
}
class BSTNode {
    Contact contact;
    BSTNode left, right;

    public BSTNode(Contact contact) {
        this.contact = contact;
        left = right = null;
    }
}
class ContactBST {
    private BSTNode root;

    public ContactBST() {
        root = null;
    }

    public void insert(Contact contact) {
        root = insertRec(root, contact);
    }

    private BSTNode insertRec(BSTNode root, Contact contact) {
        if (root == null) {
            root = new BSTNode(contact);
            return root;
        }

        if (contact.name.compareTo(root.contact.name) < 0) {
            root.left = insertRec(root.left, contact);
        } else if (contact.name.compareTo(root.contact.name) > 0) {
            root.right = insertRec(root.right, contact);
        }

        return root;
    }

    public Contact search(String name) {
        return searchRec(root, name);
    }

    private Contact searchRec(BSTNode root, String name) {
        if (root == null || root.contact.name.equals(name)) {
            return root != null ? root.contact : null;
        }

        if (root.contact.name.compareTo(name) > 0) {
            return searchRec(root.left, name);
        }

        return searchRec(root.right, name);
    }

    public void delete(String name) {
        root = deleteRec(root, name);
    }

    private BSTNode deleteRec(BSTNode root, String name) {
        if (root == null) {
            return root;
        }

        if (name.compareTo(root.contact.name) < 0) {
            root.left = deleteRec(root.left, name);
        } else if (name.compareTo(root.contact.name) > 0) {
            root.right = deleteRec(root.right, name);
        } else {
            if (root.left == null) {
                return root.right;
            } else if (root.right == null) {
                return root.left;
            }

            root.contact = minValue(root.right);
            root.right = deleteRec(root.right, root.contact.name);
        }

        return root;
    }

    private Contact minValue(BSTNode root) {
        Contact minv = root.contact;
        while (root.left != null) {
            minv = root.left.contact;
            root = root.left;
        }
        return minv;
    }

    public void inorder() {
        inorderRec(root);
    }

    private void inorderRec(BSTNode root) {
        if (root != null) {
            inorderRec(root.left);
            System.out.println(root.contact);
            inorderRec(root.right);
        }
    }
}
public class Main {
    public static void main(String[] args) {
        ContactBST contactBST = new ContactBST();

        contactBST.insert(new Contact("Amal", "1234567890", "amal@123.com"));
        contactBST.insert(new Contact("Pranav", "2345678901", "pranav@456.com"));
        contactBST.insert(new Contact("Austin", "3456789012", "austin@789.com"));

        System.out.println("Contacts in BST (in-order):");
        contactBST.inorder();

        System.out.println("\nSearching for Pranav:");
        Contact searchResult = contactBST.search("Pranav");
        if (searchResult != null) {
            System.out.println("Found: " + searchResult);
        } else {
            System.out.println("Pranav not found.");
        }

        System.out.println("\nDeleting Amal:");
        contactBST.delete("Amal");
        System.out.println("Contacts in BST (in-order) after deletion:");
        contactBST.inorder();
    }
}
