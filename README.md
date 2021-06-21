using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Inimigo : MonoBehaviour
{
    public Transform DetectaGround;
    public float distancia = 3f;
    public bool olhandoParaDireita;
	public bool olhandoParaEsquerda;
    public float velocidade = 4f; 
    public float velocidadePerseguicao = 7f;

    public bool spot = false; //booleana para saber se o jogador esta dentro do campo de visão
    public Transform target; //alvo que o inimigo vai perseguir, nesse caso o jogador
    public Transform inicioCP; //inicio do campo de visão 
    public Transform fimCP; //final do campo de visão 

    void Start()
    {
        olhandoParaDireita = true;
    }

    void Update()
    {
        Patrulha();
        Raycasting();
        Persegue();
    }

    public void Patrulha()
    {
		transform.Translate(Vector2.right * velocidade * Time.deltaTime);

        RaycastHit2D groundInfo = Physics2D.Raycast(DetectaGround.position, Vector2.down, distancia);
        if (groundInfo.collider == false)
        {
            if (olhandoParaDireita == true)
            {
                transform.eulerAngles = new Vector3(0, 180, 0);
                olhandoParaDireita = true;
            }
            else
            {
                transform.eulerAngles = new Vector3(0, 180, 0);
                olhandoParaDireita = false;
            }
			
			if (olhandoParaEsquerda == false)
            {
                transform.eulerAngles = new Vector3(0, 180, 0);
                olhandoParaEsquerda = true;
            }
            else
            {
                transform.eulerAngles = new Vector3(0, 180, 0);
                olhandoParaEsquerda = false;
            }	
        }
    }

    public void Raycasting()
    {
        Debug.DrawLine(inicioCP.position, fimCP.position, Color.green);
        spot = Physics2D.Linecast(inicioCP.position, fimCP.position, 1 << LayerMask.NameToLayer("Player"));
    }

    public void Persegue()
    {
        if (spot == true)
        {
            velocidade = velocidadePerseguicao;            
        }

        else if (spot == false)
        {
            velocidade = 4f;
        }
    }
}
