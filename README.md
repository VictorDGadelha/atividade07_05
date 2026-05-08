-- Procedure para cancelar um agendamento
CREATE OR REPLACE PROCEDURE
cancelar_agendamento(p_id INTEGER)
LANGUAGE plpgsql
AS $$
BEGIN
-- Verificar se o agendamento existe
IF NOT EXISTS (SELECT 1 FROM agendamentos
                   WHERE id = p_id) THEN
RAISE EXCEPTION
'Agendamento ID=% não encontrado!', p_id;
END IF;

-- Verificar se já está cancelado
IF EXISTS (SELECT 1 FROM agendamentos
   WHERE id = p_id AND status = 'cancelado') THEN
RAISE EXCEPTION
'Agendamento ID=% já está cancelado!', p_id;
END IF;

-- Atualizar o status para cancelado
UPDATE agendamentos
SET status = 'cancelado'
WHERE id = p_id;

RAISE NOTICE 'Agendamento ID=% cancelado
com sucesso!', p_id;
END;
$$;

-- Procedure que transfira o pet de dono
create procedure transferir_pet( in p_id_pet integer , in p_novo_dono integer) begin
	update pets 
	set id_cliente = p_novo_dono where id_pet = p_id_pet;
end $$



-- Cancelar um agendamento
CALL cancelar_agendamento(1);

-- Trocar o dono do pet
