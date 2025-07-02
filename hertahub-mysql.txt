-- ===================================================
-- 1. Tabel users
-- ===================================================
CREATE TABLE `users` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `username` VARCHAR(100) NOT NULL UNIQUE,
  `password` VARCHAR(255) NOT NULL,
  `email` VARCHAR(255) DEFAULT NULL,
  `role` ENUM('user','admin') NOT NULL DEFAULT 'user',
  `avatar` VARCHAR(255) DEFAULT NULL,
  `border` VARCHAR(255) DEFAULT NULL,
  `background` VARCHAR(255) DEFAULT NULL,
  `created_at` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
    ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- ===================================================
-- 2. Tabel threads
-- ===================================================
CREATE TABLE `threads` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `user_id` INT UNSIGNED NOT NULL,
  `title` VARCHAR(255) NOT NULL,
  `content` TEXT NOT NULL,
  `media` VARCHAR(255) DEFAULT NULL,
  `created_at` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
    ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  INDEX (`user_id`),
  CONSTRAINT `fk_threads_user`
    FOREIGN KEY (`user_id`) REFERENCES `users`(`id`)
      ON DELETE CASCADE
      ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- ===================================================
-- 3. Tabel replies
-- ===================================================
CREATE TABLE `replies` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `thread_id` INT UNSIGNED NOT NULL,
  `user_id` INT UNSIGNED NOT NULL,
  `content` TEXT NOT NULL,
  `media` VARCHAR(255) DEFAULT NULL,
  `created_at` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
    ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  INDEX (`thread_id`),
  INDEX (`user_id`),
  CONSTRAINT `fk_replies_thread`
    FOREIGN KEY (`thread_id`) REFERENCES `threads`(`id`)
      ON DELETE CASCADE
      ON UPDATE CASCADE,
  CONSTRAINT `fk_replies_user`
    FOREIGN KEY (`user_id`) REFERENCES `users`(`id`)
      ON DELETE CASCADE
      ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- ===================================================
-- 4. Tabel custom_backgrounds
-- ===================================================
CREATE TABLE `custom_backgrounds` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `filename` VARCHAR(255) NOT NULL,
  `uploaded_at` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- ===================================================
-- 5. Tabel custom_borders
-- ===================================================
CREATE TABLE `custom_borders` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `filename` VARCHAR(255) NOT NULL,
  `uploaded_at` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;


-- ===================================================
-- ALTER TABLE
-- ===================================================

ALTER TABLE users
  MODIFY email VARCHAR(255) NOT NULL,
  ADD UNIQUE (email),
  ADD COLUMN google_id VARCHAR(50) NULL;

