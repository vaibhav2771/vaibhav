CALL insert_tomcat(
  '192.168.1.100',  -- Replace with actual IP
  'New Tomcat Name',  -- Replace with new name
  8080,              -- Replace with port number
  'John Doe',         -- Replace with updated by
  'John Doe',         -- Replace with installer name
  'johndoe@example.com',  -- Replace with installer email
  'mydatabase',       -- Replace with database name
  'dbuser',           -- Replace with database username
  'dbpassword',       -- Replace with database password
  'system_user'       -- Replace with user executing the procedure
);
