# devicentemiguel
repositorio para la practica del tema 6

package calculadora;
import java.util.Scanner;
/*
Objetivo: obtener codigo refactorizado a partir de otro que no lo esta.

Tarea: Para esta tarea, se refactorizara un programa mal escrito, sin 
cambiar la forma en que funciona el programa. Este programa, RPN.java
es una calculadora de notacion inversa polaca que utiliza una pila.abstract

Reverse Polish Notation(RPN) por ejemplo
la expresion 4 * 5 - 7 / 2 % 3 no da 1,5 respetando la prioridad de 
los operadores en notacion RPN seria: 4 5 * 7 2 / -3 %(pues no podemos
poner los parentesis para alterar la prioridad)

 Se debe reorganizar este codigo usando al menos tres de las reglas 
vistas en clase.abstract
*/

class NodoPila{
	
	public NodoPila(double dato, NodoPila abajo) {
		this.dato=dato;
		this.abajo=abajo;
	}
	public NodoPila abajo;
	public double dato;
}

class RPN{
	/**clase que contiene los metodos que hacen las operaciones*/
	public void pushPila(double nuevo_dato) {
		NodoPila nuevo_nodo= new NodoPila(nuevo_dato, arriba);
		arriba=nuevo_nodo;
	}
	public double popPila() {
		double dato_arriba=arriba.dato;
		arriba=arriba.abajo;
		return dato_arriba;
	}
	public RPN(String commando) {
		arriba=null;
		this.commando=commando;
	}
	public double resultado() {
		/**metodo que hace las operaciones
		 * @param a primer operando
		 * @param b segundo operando
		 * @return val resultado de la operacion
		 * @throw IllegalArgumentException excepcion que se genera cuando se introduce un caracter ilegal o no se introduce ningun caracter
		 */
		double a, b;
		int j;
		
		for(int i=0;i<commando.length();i++) {
			//si es un digito
			if(Character.isDigit(commando.charAt(i))) {
				double numero;
				//obtener un String a partir del numero
				String temp="";
				for(j= 0; (j< 100) && (Character.isDigit(commando.charAt(i)) || (commando.charAt(i) == '.')); j++, i++) {
					temp= temp+String.valueOf(commando.charAt(i));
				}
				//convertir a double y añadir a la pila
				numero=Double.parseDouble(temp);
				pushPila(numero);				
			}else if(commando.charAt(i)=='+') {
				b=popPila();
				a=popPila();
				pushPila(a+b);
			}else if(commando.charAt(i)=='-') {
				b=popPila();
				a=popPila();
				pushPila(a-b);
			}else if(commando.charAt(i)=='*') {
				b=popPila();
				a=popPila();
				pushPila(a*b);
			}else if(commando.charAt(i)=='/') {
				b=popPila();
				a=popPila();
				pushPila(a/b);
			}else if(commando.charAt(i)=='^') {
				b=popPila();
				a=popPila();
				pushPila(Math.pow(a, b));
			}else if(commando.charAt(i)=='%') {
				b=popPila();
				a=popPila();
				pushPila(a%b);
			}else if(commando.charAt(i)!=' ') {
				throw new IllegalArgumentException();
			}
		}
		double val=popPila();
		
		if(arriba!=null) {
			throw new IllegalArgumentException();
		}
		return val;
	}
	private String commando;
	private NodoPila arriba;
}

public class TestRPN{
	/**clase principal que contendra el metodo main.
	 * como no entiendo realmente como funciona lo de la RPN, pondre los comentarios de javadoc intentando no resultar demasiado absurdo
	 * @author curso EDD
	 * @version 1.0.0
	 */
	/*metodo main*/
	public static void main(String args[]) {
		while(true) {
			Scanner in=new Scanner (System.in);
			System.out.println("introduce expresion Postfija o teclea\"fin\".");
			String linea=in.nextLine();
			if(linea.equals("fin")) {
				System.out.println("fin del programa");
				break;
			}else {
				RPN calc=new RPN(linea);
				System.out.printf("El resultado es %f\n",calc.resultado());
			}
		}
	}
}
