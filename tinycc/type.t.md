[[type.t]]
![[Pasted image 20241111204152.png]]
[[char_type]]
[[char_type.t]]

![[Pasted image 20241111204337.png]]
int_type.t=VT_INT при иницилизации компиляции ![[Pasted image 20241111204643.png]]
[[sym_push]] 
[[sym_push2]]

![[Pasted image 20241116235859.png]]
[[patch_type]]
![[Pasted image 20241117000746.png]]
![[Pasted image 20241117000826.png]]
[[static_proto]]
  if ((type->t | sym->type.t) & VT_INLINE) {

            if (!((type->t ^ sym->type.t) & VT_INLINE)

             || ((type->t | sym->type.t) & VT_STATIC))

                static_proto |= VT_INLINE;

        }
    Несовсем понятно 
    
![[Pasted image 20241117001101.png]]
[[decl_designator]]
![[Pasted image 20241117001536.png]]
[[sym->type.t]]

![[Pasted image 20241117001800.png]]
[[type.t]] далеко не только [[sym_push]] задается.
[[combine_types]]
![[Pasted image 20241117001944.png]]
![[Pasted image 20241117002037.png]]
[[t1]] и [[t2]] 
![[Pasted image 20241117002123.png]]

![[Pasted image 20241117002515.png]]
иницилизация этого [[type.t]]