import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class User {
    private String username;
    private String password;
    private String userType;

    public User(String username, String password, String userType) {
        this.username = username;
        this.password = password;
        this.userType = userType;
    }

    public String getUsername() {
        return username;
    }

    public String getPassword() {
        return password;
    }

    public String getUserType() {
        return userType;
    }

    public int getJobListingsCount() {
        return 0;
    }
}

class Job {
    private String title;
    private String description;
    private String location;
    private User employer;
    private List<User> applicants;

    public Job(String title, String description, String location, User employer) {
        this.title = title;
        this.description = description;
        this.location = location;
        this.employer = employer;
        this.applicants = new ArrayList<>();
    }

    public String getTitle() {
        return title;
    }

    public String getDescription() {
        return description;
    }

    public String getLocation() {
        return location;
    }

    public User getEmployer() {
        return employer;
    }

    public List<User> getApplicants() {
        return applicants;
    }
}

public class JobPortalApp {
    private List<User> users = new ArrayList<>();
    private List<Job> jobs = new ArrayList<>();
    private static final int MAX_EMPLOYERS = 2;
    private static final int MAX_JOB_LISTINGS = 4;
    public void registerUser(String username, String password, String userType) {
        if (users.size() >= MAX_EMPLOYERS) {
            System.out.println("Sorry, we can't register more employers.");
            return;
        }
        users.add(new User(username, password, userType));
    }

    public void postJob(String employerUsername, String title, String description, String location) {
        User employer = getUserByUsername(employerUsername);
        if (employer == null || !employer.getUserType().equalsIgnoreCase("employer")) {
            System.out.println("You must be an employer to post a job.");
            return;
        }

        if (employer.getJobListingsCount() >= MAX_JOB_LISTINGS) {
            System.out.println("You've reached the maximum job listings limit.");
            return;
        }

        Job job = new Job(title, description, location, employer);
        employer.getJobListingsCount();
        jobs.add(job);
    }

    public User getUserByUsername(String username) {
        for (User user : users) {
            if (user.getUsername().equalsIgnoreCase(username)) {
                return user;
            }
        }
        return null;
    }

    public void searchJobs(String keyword, String location) {
        for (Job job : jobs) {
            if (job.getTitle().toLowerCase().contains(keyword.toLowerCase()) && job.getLocation().toLowerCase().contains(location.toLowerCase())) {
                System.out.println("Job Title: " + job.getTitle());
                System.out.println("Posted by: " + job.getEmployer().getUsername());
                System.out.println("Location: " + job.getLocation());
                System.out.println("Description: " + job.getDescription());
                System.out.println("Number of Applicants: " + job.getApplicants().size());
                System.out.println();
            }
        }
    }

    public static void main(String[] args) {
        JobPortalApp jobPortal = new JobPortalApp();
        try (Scanner scanner = new Scanner(System.in)) {
            while (true) {
                System.out.println("1. Register\n2. Post Job\n3. Search Jobs\n4. Exit");
                int choice = scanner.nextInt();
                scanner.nextLine(); // Consume newline

                switch (choice) {
                    case 1:
                        System.out.print("Enter username: ");
                        String username = scanner.nextLine();
                        System.out.print("Enter password: ");
                        String password = scanner.nextLine();
                        System.out.print("Enter user type (employer or applicant): ");
                        String userType = scanner.nextLine();
                        jobPortal.registerUser(username, password, userType);
                        break;

                    case 2:
                        System.out.print("Enter your username: ");
                        String employerUsername = scanner.nextLine();
                        System.out.print("Enter job title: ");
                        String title = scanner.nextLine();
                        System.out.print("Enter job description: ");
                        String description = scanner.nextLine();
                        System.out.print("Enter job location: ");
                        String location = scanner.nextLine();
                        jobPortal.postJob(employerUsername, title, description, location);
                        break;

                    case 3:
                        System.out.print("Enter a keyword to search for jobs: ");
                        String keyword = scanner.nextLine();
                        System.out.print("Enter a location for the search: ");
                        String searchLocation = scanner.nextLine();
                        jobPortal.searchJobs(keyword, searchLocation);
                        break;

                    case 4:
                        System.out.println("Goodbye!");
                        System.exit(0);
                }
            }
        }
    }
}
