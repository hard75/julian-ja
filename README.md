julian-ja
=========

Consulta base de datos

package mysqlbd;
import java.sql.*;

public class MySQLBD {
   
private Connection conexion;


public Connection getConexion() {
    return conexion;
} 

public void setConexion(Connection conexion) {
        this.conexion = conexion;
}   
public MySQLBD conectar() {
 
    try {
    Class.forName("com.mysql.jdbc.Driver");
    String BaseDeDatos = "jdbc:mysql://localhost/repositorio?use=root&password=";
    setConexion(DriverManager.getConnection(BaseDeDatos));
    if(getConexion() != null){
                System.out.println("Conexion Exitosa!");
            }else{
                System.out.println("Conexion Fallida!");               
            }
   
 } catch (ClassNotFoundException | SQLException e) {
    }
        
    return this;
 }

public boolean ejecutar(String sql) {
        try {
            try (Statement sentencia = getConexion().createStatement(ResultSet.TYPE_FORWARD_ONLY, ResultSet.CONCUR_READ_ONLY)) {
                sentencia.executeUpdate(sql);
            }
        } catch (SQLException e) {
            e.printStackTrace();
            return false;
        }        return true;
    }
public ResultSet consultar(String sql) {
        ResultSet resultado;
        try {
            Statement sentencia = getConexion().createStatement(ResultSet.TYPE_FORWARD_ONLY, ResultSet.CONCUR_READ_ONLY);
            resultado = sentencia.executeQuery(sql);
        } catch (SQLException e) {
            e.printStackTrace();
            return null;
        }        return resultado;
    }

    public static void main(String[] args) {
        // TODO code application logic here
        
         MySQLBD baseDatos = new MySQLBD().conectar();
         
        if (baseDatos.ejecutar("INSERT INTO usuarios(Id,Nombre,Apellido) VALUES(1,'julian,'ja')")) {
            System.out.println("Ejecución correcta!");
        } else {
            System.out.println("El usuario ya se encuentra en la base de datos");
        }        
        
        ResultSet resultados = baseDatos.consultar("SELECT * FROM usuarios");      
        
        if (resultados != null) {
            try {
                System.out.println("Id      Nombre    Apellido ");
                System.out.println("--------------------------------");
                while (resultados.next()) {
                    System.out.println(""+resultados.getInt("Id")+"       "+resultados.getString("Nombre")+resultados.getString("Apellido"));
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        
    }
    
}
