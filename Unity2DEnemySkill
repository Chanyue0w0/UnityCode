using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class EMovingSkill : MonoBehaviour
{
    //GameObject player_1;
    //protected player1DA player1;

    public Rigidbody2D rb;

    public char DeBuff = 'n';
    public float DeBuffTime;
    public int FaceDirection;
    public float MoveSpeed = 500;
    public float JumpForce = 0;
    public float Damage = 200;

    //擊退,擊飛先用transform寫,不確定用velocity是否會被Player的程式歸零
    public float KnockDistance = 0f;//擊退距離 
    public float KnockHight = 0f;//擊飛高度
    public bool FaceDireKnock = false;//技能面向方向擊退

    public bool JumpingMove = false;//是否為跳躍移動行技能
    public bool OnTrigD = false;//碰撞or觸發就自毀
    public bool DExplosion = false;//自毀後產生爆炸
    public float DTime = 2f;

    public GameObject Explosion;
    public GameObject Explosion2;

    private void Awake()
    {
        //player_1 = GameObject.FindGameObjectWithTag("Player");
    }

    void Start()
    {

        Damage *= GameManager.Level;
        MoveSpeed *= (GameManager.Level / 10 + 1);//速度增加等級之10%

        if (gameObject.transform.rotation.y < 0)//向左
            FaceDirection = -1;
        else
            FaceDirection = 1;
    }


    void FixedUpdate()
    {
        if (gameObject.transform.rotation.y < 0)//向左
            FaceDirection = -1;
        else
            FaceDirection = 1;


        rb.velocity = new Vector2(FaceDirection * MoveSpeed * Time.deltaTime, rb.velocity.y);

        DTime -= Time.deltaTime;

        if(DTime <= 0)
        {
            if(Explosion != null && DExplosion == true)
                Instantiate(Explosion, gameObject.transform.localPosition, Quaternion.identity);

            Destroy(gameObject);

        }
        



    }

    private void OnTriggerEnter2D(Collider2D collision)
    {
        if(collision.CompareTag("Player"))//技能Trigger到玩家    && !GameManager.isDashing
        {
            //Debug.Log("Hit");
            GameManager.Damage = Damage;//造成傷害
            GameManager.Condition = DeBuff;
            GameManager.ConditionTime = DeBuffTime;

            if(Explosion != null)
                Instantiate(Explosion, collision.transform.localPosition, Quaternion.identity);

            if(Explosion2 != null)
                Instantiate(Explosion2, collision.transform.localPosition, Quaternion.identity);


            if ( (KnockDistance > 0 || KnockHight > 0) && !FaceDireKnock)//若有 擊退or擊飛 效果
            {
                if(collision.transform.position.x <= gameObject.transform.position.x)//玩家在技能左方 , 向左擊退
                {
                    collision.transform.position = new Vector2(collision.transform.position.x - KnockDistance * Time.deltaTime, collision.transform.position.y + KnockHight * Time.deltaTime);
                }
                else//玩家在技能右方 , 向右擊退
                {
                    collision.transform.position = new Vector2(collision.transform.position.x + KnockDistance * Time.deltaTime, collision.transform.position.y + KnockHight * Time.deltaTime);
                }
                
            }
            else if ( (KnockDistance > 0 || KnockHight > 0) && FaceDireKnock)//若有 擊退or擊飛 效果 (旋轉方向擊退)
            {
                if(gameObject.transform.rotation.y < 0)
                    collision.transform.position = new Vector2(collision.transform.position.x - KnockDistance * Time.deltaTime, collision.transform.position.y + KnockHight * Time.deltaTime);
                else
                    collision.transform.position = new Vector2(collision.transform.position.x + KnockDistance * Time.deltaTime, collision.transform.position.y + KnockHight * Time.deltaTime);
                //用FaceDirection控制方向 (不可) ,不知為何攻擊瞬間時FaceDirection = 0
            }

            

            if (OnTrigD)
                Destroy(gameObject);
        }

        /*if(collision.CompareTag("Ground"))
        {
            Destroy(gameObject);
        }*/
    }

    private void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ground") && JumpingMove)
        {
            rb.velocity = new Vector2(FaceDirection * MoveSpeed * Time.deltaTime, JumpForce * Time.deltaTime);
        }
    }

}
