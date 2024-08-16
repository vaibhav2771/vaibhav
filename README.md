

DELIMITER $$
CREATE PROCEDURE insert_tomcat(
  IN p_ip VARCHAR(255),
  IN p_newname VARCHAR(255),
  IN p_portnumber INT,
  IN p_updatedby VARCHAR(255),
  IN p_instname VARCHAR(255),
  IN p_instemail VARCHAR(255),
  IN p_dbname VARCHAR(255),
  IN p_dbuname VARCHAR(255),
  IN p_dbpass VARCHAR(255),
  IN p_executed_by VARCHAR(255)
)
BEGIN
  DECLARE v_appid INT;
  DECLARE v_podid VARCHAR(255);
  DECLARE v_tid INT;

  -- Fetch appid and podid from appservers
  SELECT appid, podid INTO v_appid, v_podid FROM appservers WHERE ip = p_ip;

  -- Insert into tomcats
  INSERT INTO tomcats (appid, podid, newname, portnumber, updatedby, tomcatlocation, used, emrversionid, active)
  VALUES (v_appid, v_podid, p_newname, p_portnumber, p_updatedby, '/* tomcatlocation value */', '/* used value */', '/* emrversionid value */', '/* active value */');

  -- Get the last inserted tid from tomcats
  SET v_tid = LAST_INSERT_ID();

  -- Insert into renameaction
  INSERT INTO renameaction (tid, newname, appid, ip, /* other columns */)
  VALUES (v_tid, p_newname, v_appid, p_ip, /* other values */, p_instname, p_instemail, p_dbname, p_dbuname, p_dbpass);

  -- Log the stored procedure execution
  INSERT INTO sp_logs (sp_name, executed_by, executed_on, parameters)
  VALUES ('insert_tomcat', p_executed_by, NOW(), CONCAT('ip:', p_ip, ', newname:', p_newname, ', portnumber:', p_portnumber, ', updatedby:', p_updatedby));
END $$
DELIMITER ;
