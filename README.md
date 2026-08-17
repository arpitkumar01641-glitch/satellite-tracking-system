# satellite-tracking-system
tracking and locating
import java.util.*;

class Satellite {
    int id;
    String name;
    String orbitType; // LEO, MEO, GEO
    double altitudeKm;
    String status; // Active, Inactive, Decommissioned
    int fuelPercent;

    Satellite(int id, String name, String orbitType, double altitudeKm, int fuelPercent) {
        this.id = id;
        this.name = name;
        this.orbitType = orbitType;
        this.altitudeKm = altitudeKm;
        this.fuelPercent = fuelPercent;
        this.status = "Active";
    }

    void display() {
        String alert = (fuelPercent <= 15) ? " ⚠ LOW FUEL!" : "";
        System.out.println("ID: " + id + " | Name: " + name + " | Orbit: " + orbitType
                + " | Altitude: " + altitudeKm + " km | Fuel: " + fuelPercent + "%"
                + " | Status: " + status + alert);
    }
}

public class SatelliteTrackingSystem {
    static List<Satellite> satellites = new ArrayList<>();
    static Scanner sc = new Scanner(System.in);
    static int nextId = 1;

    public static void main(String[] args) {
        int choice;
        do {
            printMenu();
            choice = Integer.parseInt(sc.nextLine());
            switch (choice) {
                case 1 -> addSatellite();
                case 2 -> displayAll();
                case 3 -> filterByOrbit();
                case 4 -> updateFuel();
                case 5 -> decommission();
                case 6 -> lowFuelAlerts();
                case 0 -> System.out.println("Exiting... Mission control signing off.");
                default -> System.out.println("Invalid choice.");
            }
        } while (choice != 0);
    }

    static void printMenu() {
        System.out.println("\n===== SATELLITE TRACKING & MONITORING SYSTEM =====");
        System.out.println("1. Add Satellite");
        System.out.println("2. Display All Satellites");
        System.out.println("3. Filter by Orbit Type (LEO/MEO/GEO)");
        System.out.println("4. Update Fuel Level");
        System.out.println("5. Decommission Satellite");
        System.out.println("6. Check Low Fuel Alerts");
        System.out.println("0. Exit");
        System.out.print("Enter choice: ");
    }

    static void addSatellite() {
        System.out.print("Enter Name: ");
        String name = sc.nextLine();
        System.out.print("Enter Orbit Type (LEO/MEO/GEO): ");
        String orbit = sc.nextLine().toUpperCase();
        System.out.print("Enter Altitude (km): ");
        double alt = Double.parseDouble(sc.nextLine());
        System.out.print("Enter Fuel Percent: ");
        int fuel = Integer.parseInt(sc.nextLine());

        satellites.add(new Satellite(nextId++, name, orbit, alt, fuel));
        System.out.println("Satellite added and now tracked.");
    }

    static void displayAll() {
        if (satellites.isEmpty()) {
            System.out.println("No satellites being tracked.");
            return;
        }
        for (Satellite s : satellites) s.display();
    }

    static void filterByOrbit() {
        System.out.print("Enter Orbit Type to filter (LEO/MEO/GEO): ");
        String orbit = sc.nextLine().toUpperCase();
        boolean found = false;
        for (Satellite s : satellites) {
            if (s.orbitType.equals(orbit)) {
                s.display();
                found = true;
            }
        }
        if (!found) System.out.println("No satellites found in " + orbit + " orbit.");
    }

    static void updateFuel() {
        System.out.print("Enter Satellite ID: ");
        int id = Integer.parseInt(sc.nextLine());
        for (Satellite s : satellites) {
            if (s.id == id) {
                System.out.print("Enter new fuel percent: ");
                s.fuelPercent = Integer.parseInt(sc.nextLine());
                System.out.println("Fuel level updated.");
                if (s.fuelPercent <= 15) {
                    System.out.println("⚠ ALERT: " + s.name + " is critically low on fuel!");
                }
                return;
            }
        }
        System.out.println("Satellite ID not found.");
    }

    static void decommission() {
        System.out.print("Enter Satellite ID to decommission: ");
        int id = Integer.parseInt(sc.nextLine());
        for (Satellite s : satellites) {
            if (s.id == id) {
                s.status = "Decommissioned";
                System.out.println(s.name + " marked as decommissioned.");
                return;
            }
        }
        System.out.println("Satellite ID not found.");
    }

    static void lowFuelAlerts() {
        boolean any = false;
        System.out.println("--- LOW FUEL SATELLITES ---");
        for (Satellite s : satellites) {
            if (s.fuelPercent <= 15 && !s.status.equals("Decommissioned")) {
                s.display();
                any = true;
            }
        }
        if (!any) System.out.println("All active satellites have sufficient fuel.");
    }
}
