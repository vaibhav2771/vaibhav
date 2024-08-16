DELIMITER $$
CREATE PROCEDURE insert_QATomcats(
  IN p_ip VARCHAR(255),
  IN p_newname VARCHAR(255),
  IN p_portnumber INT,
  IN p_instname VARCHAR(255),
  IN p_instemail VARCHAR(255),
  IN p_dbname VARCHAR(255),
  IN p_dbuname VARCHAR(255),
  IN p_dbpass VARCHAR(255),
  IN p_executed_by VARCHAR(255)
  
)
BEGIN
  DECLARE v_appid INT;
  DECLARE v_podid INT;
  DECLARE v_tid INT;
  DECLARE p_tomcatname VARCHAR(20);
  DECLARE p_tomcatrename VARCHAR(20);
  DECLARE p_message VARCHAR (100);
SELECT id, podid INTO v_appid, v_podid FROM appservers WHERE ip = p_ip;
SELECT count(newname) INTO p_tomcatname FROM tomcats WHERE newname = p_newname AND appid = v_appid;
SELECT count(newname) INTO p_tomcatrename FROM renameaction WHERE newname = p_newname AND appid = v_appid;
IF p_tomcatname =0 THEN 

IF p_tomcatrename =0 THEN
  -- Fetch appid and podid from appservers
  
  
  -- Insert into tomcats
  INSERT INTO tomcats (appid, podid, newname, portno, updatedby, tomcatlocation, used, emrversionid, ACTIVE)
  VALUES (v_appid, v_podid, p_newname, p_portnumber, p_instname, '/eClinicalWorks/prodapps/', '1', '10', '1');
  -- Get the last inserted tid from tomcats
  SET v_tid = LAST_INSERT_ID();

  -- Insert into renameaction
  INSERT INTO renameaction (tid, newname, appid, ip, instname,instemail,dbname,dbuname,dbpass,complete,vid,checked)
  VALUES (v_tid, p_newname, v_appid, p_ip, p_instname, p_instemail, p_dbname, p_dbuname, p_dbpass,1,10,1);
  ELSE
  SET p_message = 'Tomcat is already present in renameaction table';
  END IF;
  ELSE
  SET p_message = 'Tomcat is already present in tomcat table';
  END IF;
  -- Log the stored procedure execution
  INSERT INTO storedproc_logs (sp_name, executed_by, executed_on, TomcatName, PortNumber, IP,message)
  VALUES ('insert_QATomcats', p_executed_by, NOW(),p_newname,p_portnumber,p_ip,p_message);
END $$
DELIMITER ;


CALL insert_QATomcats(
  '172.20.3.16',  -- Replace with actual IP
  'tomcat_zzzzzz',  -- Replace with new name
  '8888',              -- Replace with port number
  'Dinesh Nair',         -- Replace with installer name
  'Dinesh.Nair@eclinicalworks',  -- Replace with installer email
  'cccccc',       -- Replace with database name
  'ecwdbuser',           -- Replace with database username
  'xxxxxxxx',       -- Replace with database password
  'Dinesh Nair'  -- Replace with user executing the procedure    
);

