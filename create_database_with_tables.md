-- Create the database if it doesn't exist
CREATE DATABASE IF NOT EXISTS `tours_and_travels`;
USE `tours_and_travels`;

-- --------------------------------------------------------

--
-- Table structure for table `users`
--

CREATE TABLE `users` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `email` varchar(255) NOT NULL,
  `password` varchar(255) NOT NULL,
  `role` enum('admin','customer') NOT NULL DEFAULT 'customer',
  `created_at` datetime NOT NULL DEFAULT current_timestamp(),
  `updated_at` datetime DEFAULT NULL ON UPDATE current_timestamp(),
  PRIMARY KEY (`id`),
  UNIQUE KEY `email` (`email`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- --------------------------------------------------------

--
-- Table structure for table `packages`
--

CREATE TABLE `packages` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT,
  `title` varchar(255) NOT NULL,
  `destination` varchar(255) DEFAULT NULL,
  `duration` varchar(100) DEFAULT NULL,
  `price_per_person` decimal(10,2) NOT NULL,
  `discount` int(3) DEFAULT 0,
  `category` varchar(100) DEFAULT NULL,
  `description` text DEFAULT NULL,
  `itinerary` text DEFAULT NULL,
  `inclusions` text DEFAULT NULL,
  `exclusions` text DEFAULT NULL,
  `hotel_details` text DEFAULT NULL,
  `transport_details` text DEFAULT NULL,
  `image` varchar(255) DEFAULT NULL,
  `image_urls` text DEFAULT NULL,
  `start_date` date DEFAULT NULL,
  `end_date` date DEFAULT NULL,
  `status` enum('Active','Inactive') NOT NULL DEFAULT 'Active',
  `created_at` datetime NOT NULL DEFAULT current_timestamp(),
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- --------------------------------------------------------

--
-- Table structure for table `bookings`
--

CREATE TABLE `bookings` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT,
  `user_id` int(11) UNSIGNED NOT NULL,
  `package_id` int(11) UNSIGNED NOT NULL,
  `tour_date` date DEFAULT NULL,
  `num_persons` int(11) NOT NULL,
  `contact_phone` varchar(20) NOT NULL,
  `health_discomforts` text DEFAULT NULL,
  `total_price` decimal(10,2) NOT NULL,
  `created_at` datetime NOT NULL DEFAULT current_timestamp(),
  `status` varchar(50) NOT NULL DEFAULT 'Confirmed',
  PRIMARY KEY (`id`),
  KEY `user_id` (`user_id`),
  KEY `package_id` (`package_id`),
  CONSTRAINT `bookings_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`) ON DELETE CASCADE,
  CONSTRAINT `bookings_ibfk_2` FOREIGN KEY (`package_id`) REFERENCES `packages` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- --------------------------------------------------------

--
-- Table structure for table `settings`
--

CREATE TABLE `settings` (
  `id` int(11) UNSIGNED NOT NULL AUTO_INCREMENT,
  `setting_key` varchar(255) NOT NULL,
  `setting_value` text DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `setting_key` (`setting_key`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- --------------------------------------------------------

--
-- Dumping data for tables
--

-- Insert a default admin user
-- Password is 'password123'
INSERT INTO `users` (`name`, `email`, `password`, `role`) VALUES
('Admin User', 'admin@example.com', '$2y$10$9.p2v9wX2C8i3E/xR4X4E.2G4U2R2I6o3E/xR4X4E.2G4U2R2I6o3', 'admin');

-- Insert a default setting for the landing page background
INSERT INTO `settings` (`setting_key`, `setting_value`) VALUES
('landing_page_background', 'https://placehold.co/1920x1080/EBF4FF/333333?text=Explore+the+World');


ALTER TABLE `packages`
ADD `tags` VARCHAR(255) NULL DEFAULT NULL AFTER `category`;