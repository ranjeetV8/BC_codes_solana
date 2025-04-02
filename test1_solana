use anchor_lang::prelude::*;

declare_id!("84zXpkmWbyQ7pwAg2cKQX3G4MUEFFiWLVon6Uooq2bu4");

pub const ANCHOR_DISCRIMINATOR_SIZE: usize = 8;

#[program]
pub mod favorites {
    use super::*;

    pub fn set_favorites(
        context: Context<SetFavorites>,
        number: u64,
        color: String,
        hobbies: Vec<String>,
    ) -> Result<()> {
        msg!("Greeting from {}", context.program_id);

        let user_public_key = context.accounts.user.key();

        msg!(
            "User {}'s favorite number is {}, color is {}, and their hobbies are {:?}",
            user_public_key, number, color, hobbies
        );

        context.accounts.test.set_inner(Test {
            number,
            color,
            hobbies,
        });

        Ok(())
    }
}

#[Account]
pub struct Test {
    pub number: u64,

    #[max_len(50)]
    pub color: String,

    #[max_len(5, 50)]  
    pub hobbies: Vec<String>,
}

#[derive(Accounts)]
pub struct SetFavorites<'info> {
    #[account(mut)]
    pub user: Signer<'info>,

    #[account(
        init_if_needed,
        payer = user,
        space = ANCHOR_DISCRIMINATOR_SIZE + 8 + (50 + 4) + (5 * (50 + 4)), // Correct space calculation
        seeds = [b"test", user.key().as_ref()],
        bump
    )]
    pub test: Account<'info, Test>,

    pub system_program: Program<'info, System>,
}
