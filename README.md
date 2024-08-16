DELIMITER $$

CREATE PROCEDURE insert_QATomcats(
  -- parameters
)
BEGIN
  DECLARE v_appid INT;
  DECLARE v_podid INT;
  DECLARE v_tid INT;
  DECLARE p_tomcatname VARCHAR(20);
  DECLARE p_tomcatrename VARCHAR(20);
  DECLARE @message VARCHAR(255);

  -- Your existing code here

  IF EXISTS (SELECT 1 FROM renameaction WHERE newname = p_newname) THEN
    SET @message = 'Tomcat is already present in renameaction table';
  ELSEIF EXISTS (SELECT 1 FROM tomcats WHERE newname = p_newname) THEN
    SET @message = 'Tomcat is already present in tomcat table';
  ELSE
    -- Insert logic here if neither condition is true
  END IF;

  -- Additional code if needed

END $$

DELIMITER ;
